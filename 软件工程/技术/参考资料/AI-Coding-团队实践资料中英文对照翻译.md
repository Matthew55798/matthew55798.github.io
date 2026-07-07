# AI Coding 团队实践资料中英文对照翻译

> 本文档汇集了 26 篇 AI Coding / Vibe Coding 团队实践相关的网络资料，采用中英文交替对照翻译格式。
>
> 翻译范围：仅网络资料，不含任何本地文档。
>
> 格式说明：每篇资料以 `## 资料标题` 开始，标注 `原文链接`。正文采用"几行英文 + 几行中文"的交替对照格式，代码块、命令、路径、API 名称、配置字段保持原文不翻译。

---

## 目录

### Codex 系列（9 篇）

1. [Codex - Sandbox（沙箱）](#codex---sandbox沙箱)
2. [Codex - Prompting（提示词）](#codex---prompting提示词)
3. [Codex - Workflows（工作流）](#codex---workflows工作流)
4. [Codex - Best practices（最佳实践）](#codex---best-practices最佳实践)
5. [Codex - Custom instructions with AGENTS.md（用 AGENTS.md 自定义指令）](#codex---custom-instructions-with-agentsmd)
6. [Codex - Agent Skills（智能体技能）](#codex---agent-skills智能体技能)
7. [Codex - Subagents（子代理）](#codex---subagents子代理)
8. [Codex - Code review in GitHub（GitHub 代码审查）](#codex---code-review-in-github)
9. [Codex - CLI features（CLI 功能）— 访问失败](#codex---cli-features-访问失败)

### Claude Code 系列（7 篇）

10. [Claude Code - Automate actions with hooks（用 hook 自动化操作）](#claude-code---automate-actions-with-hooks)
11. [Claude Code - Common workflows（常用工作流）](#claude-code---common-workflows)
12. [Claude Code - How Claude remembers your project（Claude 如何记忆你的项目）](#claude-code---how-claude-remembers-your-project)
13. [Claude Code - Power user tips（进阶用户技巧）](#claude-code---power-user-tips)
14. [Claude Code - Extend Claude with skills（用 skill 扩展 Claude）](#claude-code---extend-claude-with-skills)
15. [Claude Code - Subagents（子代理）](#claude-code---subagents)
16. [Claude Code - GitHub Actions](#claude-code---github-actions)

### OpenCode 系列（3 篇）

17. [OpenCode - Introduction（简介）](#opencode---introduction)
18. [OpenCode - Agents（智能体）](#opencode---agents)
19. [OpenCode - Configuration（配置）](#opencode---configuration)

### 通用工程实践系列（7 篇）

20. [Google - Code Review（代码审查）](#google---code-review)
21. [Google - Small CLs（小变更）](#google---small-cls)
22. [DORA - Accelerate State of DevOps 2025](#dora---accelerate-state-of-devops-2025)
23. [OWASP Top 10](#owasp-top-10)
24. [GitHub Copilot - Repository Instructions（仓库指令）](#github-copilot---repository-instructions)
25. [GitHub Copilot - Code Review（代码审查）](#github-copilot---code-review)
26. [Responsible AI（负责任的 AI）](#responsible-ai)

---
---


## Codex - Sandbox（沙箱）

原文链接：https://developers.openai.com/codex/concepts/sandboxing

### Sandbox

The sandbox is the boundary that lets Codex act autonomously without giving it unrestricted access to your machine. When Codex runs local commands in the **Codex app**, **IDE extension**, or **CLI**, those commands run inside a constrained environment instead of running with full access by default.

sandbox（沙箱）是一个边界，它让 Codex 能够自主行动，同时不会赋予它对你机器的完全访问权限。当 Codex 在 **Codex 应用**、**IDE 扩展**或 **CLI（命令行界面）** 中运行本地命令时，这些命令会在受限环境中执行，而不是默认以完全权限运行。

That environment defines what Codex can do on its own, such as which files it can modify and whether commands can use the network. When a task stays inside those boundaries, Codex can keep moving without stopping for confirmation. When it needs to go beyond them, Codex falls back to the approval flow.

该环境定义了 Codex 可以自行做哪些事情，例如能修改哪些文件、命令能否使用网络。当任务保持在这些边界之内时，Codex 可以持续推进，无需停下来等待确认。当它需要越过这些边界时，Codex 会回退到 approval（审批）流程。

Sandboxing and approvals are different controls that work together. The sandbox defines technical boundaries. The approval policy decides when Codex must stop and ask before crossing them.

沙箱机制与审批是两种不同的控制手段，二者协同工作。sandbox 定义技术边界，approval policy（审批策略）决定 Codex 何时必须停下来、在越过边界前向你请示。

### What the sandbox does

The sandbox applies to spawned commands, not just to Codex's built-in file operations. If Codex runs tools like `git`, package managers, or test runners, those commands inherit the same sandbox boundaries.

### 沙箱的作用

沙箱对派生的命令生效，而不仅仅针对 Codex 内置的文件操作。如果 Codex 运行 `git`、包管理器或测试运行器等工具，这些命令会继承相同的沙箱边界。

Codex uses platform-native enforcement on each OS. The implementation differs between macOS, Linux, WSL2, and native Windows, but the idea is the same across surfaces: give the agent a bounded place to work so routine tasks can run autonomously inside clear limits.

Codex 在每个操作系统上使用平台原生的强制机制。macOS、Linux、WSL2 和原生 Windows 上的实现各不相同，但核心理念在各个平台上是一致的：为 agent（智能体）提供一个有边界的工作空间，使常规任务能在明确的限制内自主运行。

### Why it matters

The sandbox reduces approval fatigue. Instead of asking you to confirm every low-risk command, Codex can read files, make edits, and run routine project commands within the boundary you already approved.

### 为什么这很重要

沙箱可以减少审批疲劳。Codex 不必让你逐条确认每一个低风险命令，而是可以在你已批准的边界内读取文件、进行编辑并运行常规项目命令。

It also gives you a clearer trust model for agentic work. You aren't just trusting the agent's intentions; you are trusting that the agent is operating inside enforced limits. That makes it easier to let Codex work independently while still knowing when it will stop and ask for help.

它还为智能体工作提供了更清晰的信任模型。你不仅是在信任智能体的意图，更是在信任智能体在强制限制下运行。这让你更容易放心地让 Codex 独立工作，同时仍然知道它何时会停下来寻求帮助。

### Getting started

Codex applies sandboxing automatically when you use the default permissions mode.

### 入门

当你使用默认权限模式时，Codex 会自动应用沙箱机制。

### Prerequisites

On **macOS**, sandboxing works out of the box using the built-in Seatbelt framework.

### 前置条件

在 **macOS** 上，沙箱功能开箱即用，使用内置的 Seatbelt 框架。

On **Windows**, Codex uses the native [Windows sandbox](https://developers.openai.com/codex/windows#windows-sandbox) when you run in PowerShell and the Linux sandbox implementation when you run in WSL2.

在 **Windows** 上，当你在 PowerShell 中运行时，Codex 使用原生的 [Windows sandbox](https://developers.openai.com/codex/windows#windows-sandbox)；当你在 WSL2 中运行时，则使用 Linux 沙箱实现。

On **Linux and WSL2**, install `bubblewrap` with your package manager first:

在 **Linux 和 WSL2** 上，请先使用包管理器安装 `bubblewrap`：

```bash
sudo apt install bubblewrap
```

```bash
sudo dnf install bubblewrap
```

（以上分别为 Ubuntu/Debian 和 Fedora 系统的安装命令。）

Codex uses the first `bwrap` executable it finds on `PATH`. If no `bwrap` executable is available, Codex falls back to a bundled helper, but that helper requires support for unprivileged user namespace creation. Installing the distribution package that provides `bwrap` keeps this setup reliable.

Codex 会使用它在 `PATH` 中找到的第一个 `bwrap` 可执行文件。如果没有可用的 `bwrap`，Codex 会回退到内置的辅助程序，但该辅助程序需要系统支持非特权用户命名空间的创建。安装提供 `bwrap` 的发行版软件包可以让这套设置更加可靠。

Codex surfaces a startup warning when `bwrap` is missing or when the helper can't create the needed user namespace. On distributions that restrict this AppArmor setting, you can enable it with:

当 `bwrap` 缺失或辅助程序无法创建所需的用户命名空间时，Codex 会在启动时发出警告。在限制此 AppArmor 设置的发行版上，你可以通过以下命令启用它：

```bash
sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0
```

### How you control it

Most people start with the permissions controls in the product.

### 如何进行控制

大多数人会从产品中的权限控制开始使用。

In the Codex app and IDE, you choose a mode from the permissions selector under the composer or chat input. That selector lets you rely on Codex's default permissions, switch to full access, or use your custom configuration.

在 Codex 应用和 IDE 中，你可以从输入框或聊天输入下方的权限选择器中选择一种模式。该选择器让你可以使用 Codex 的默认权限、切换到完全访问，或使用你的自定义配置。

（此处原页面包含一张权限选择器的截图，展示 Default permissions、Full access 和 Custom (config.toml) 三个选项。）

In the CLI, use [`/permissions`](https://developers.openai.com/codex/cli/slash-commands#update-permissions-with-permissions) to switch modes during a session.

在 CLI 中，使用 [`/permissions`](https://developers.openai.com/codex/cli/slash-commands#update-permissions-with-permissions) 在会话过程中切换模式。

### Configure defaults

If you want Codex to start with the same behavior every time, use a custom configuration. Codex stores those defaults in `config.toml`, its local settings file. [Config basics](https://developers.openai.com/codex/config-basic) explains how it works, and the [Configuration reference](https://developers.openai.com/codex/config-reference) documents the exact keys for `sandbox_mode`, `approval_policy`, and `sandbox_workspace_write.writable_roots`. Use those settings to decide how much autonomy Codex gets by default, which directories it can write to, and when it should pause for approval.

### 配置默认值

如果你希望 Codex 每次启动时都保持相同的行为，请使用自定义配置。Codex 将这些默认值存储在本地设置文件 `config.toml` 中。[Config basics](https://developers.openai.com/codex/config-basic) 说明了其工作原理，[Configuration reference](https://developers.openai.com/codex/config-reference) 文档记录了 `sandbox_mode`、`approval_policy` 和 `sandbox_workspace_write.writable_roots` 的确切键值。使用这些设置来决定 Codex 默认获得多大的自主权、可以写入哪些目录，以及何时应该暂停等待审批。

At a high level, the common sandbox modes are:

- `read-only`: Codex can inspect files, but it can't edit files or run commands without approval.
- `workspace-write`: Codex can read files, edit within the workspace, and run routine local commands inside that boundary. This is the default low-friction mode for local work.
- `danger-full-access`: Codex runs without sandbox restrictions. This removes the filesystem and network boundaries and should be used only when you want Codex to act with full access.

概括来说，常见的沙箱模式有：

- `read-only`：Codex 可以查看文件，但不能编辑文件或在未经审批的情况下运行命令。
- `workspace-write`：Codex 可以读取文件、在工作区内编辑，并在该边界内运行常规本地命令。这是本地工作的默认低摩擦模式。
- `danger-full-access`：Codex 在无沙箱限制下运行。这会移除文件系统和网络边界，仅在你希望 Codex 以完全权限行动时使用。

The common approval policies are:

- `untrusted`: Codex asks before running commands that aren't in its trusted set.
- `on-request`: Codex works inside the sandbox by default and asks when it needs to go beyond that boundary.
- `never`: Codex doesn't stop for approval prompts.

常见的审批策略有：

- `untrusted`：Codex 在运行不在其受信集合中的命令前会先询问。
- `on-request`：Codex 默认在沙箱内工作，需要越过边界时才会询问。
- `never`：Codex 不会因审批提示而暂停。

Full access means using `sandbox_mode = "danger-full-access"` together with `approval_policy = "never"`. By contrast, `--full-auto` is the lower-risk local automation preset: `sandbox_mode = "workspace-write"` and `approval_policy = "on-request"`.

完全访问意味着将 `sandbox_mode = "danger-full-access"` 与 `approval_policy = "never"` 配合使用。相比之下，`--full-auto` 是风险较低的本地自动化预设：`sandbox_mode = "workspace-write"` 且 `approval_policy = "on-request"`。

If you need Codex to work across more than one directory, writable roots let you extend the places it can modify without removing the sandbox entirely. If you need a broader or narrower trust boundary, adjust the default sandbox mode and approval policy instead of relying on one-off exceptions.

如果你需要 Codex 在多个目录间工作，writable roots（可写根目录）让你可以扩展它能修改的位置，而无需完全移除沙箱。如果你需要更宽或更窄的信任边界，请调整默认的沙箱模式和审批策略，而不是依赖一次性的例外。

For reusable permission sets, set `default_permissions` to a named profile and define `[permissions.<name>.filesystem]` or `[permissions.<name>.network]`. Managed network profiles use map tables such as `[permissions.<name>.network.domains]` and `[permissions.<name>.network.unix_sockets]` for domain and socket rules.

对于可复用的权限集，可以将 `default_permissions` 设置为一个命名 profile（配置档案），并定义 `[permissions.<name>.filesystem]` 或 `[permissions.<name>.network]`。托管网络 profile 使用映射表（如 `[permissions.<name>.network.domains]` 和 `[permissions.<name>.network.unix_sockets]`）来配置域名和 socket 规则。

When a workflow needs a specific exception, use [rules](https://developers.openai.com/codex/rules). Rules let you allow, prompt, or forbid command prefixes outside the sandbox, which is often a better fit than broadly expanding access. For a higher-level overview of approvals and sandbox behavior in the app, see [Codex app features](https://developers.openai.com/codex/app/features#approvals-and-sandboxing), and for the IDE-specific settings entry points, see [Codex IDE extension settings](https://developers.openai.com/codex/ide/settings).

当某个 workflow（工作流）需要特定例外时，请使用 [rules](https://developers.openai.com/codex/rules)。rules（规则）让你可以在沙箱之外允许、提示或禁止特定命令前缀，这通常比大幅扩展访问权限更合适。如需了解应用中审批和沙箱行为的更高层概览，请参阅 [Codex app features](https://developers.openai.com/codex/app/features#approvals-and-sandboxing)；如需 IDE 专属的设置入口，请参阅 [Codex IDE extension settings](https://developers.openai.com/codex/ide/settings)。

Platform details live in the platform-specific docs. For native Windows setup, behavior, and troubleshooting, see [Windows](https://developers.openai.com/codex/windows). For admin requirements and organization-level constraints on sandboxing and approvals, see [Agent approvals & security](https://developers.openai.com/codex/agent-approvals-security).

平台相关的细节位于各平台专属文档中。如需原生 Windows 的设置、行为和故障排除，请参阅 [Windows](https://developers.openai.com/codex/windows)。如需了解管理员要求和组织级别的沙箱与审批约束，请参阅 [Agent approvals & security](https://developers.openai.com/codex/agent-approvals-security)。

---

## Codex - Prompting（提示词）

原文链接：https://developers.openai.com/codex/prompting

### Prompts

You interact with Codex by sending prompts (user messages) that describe what you want it to do.

你通过发送 prompt（提示词，即用户消息）来与 Codex 交互，描述你希望它完成的工作。

When you submit a prompt, Codex works in a loop: it calls the model and then performs the actions indicated by the model output, such as file reads, file edits, and tool calls. This process ends when the task is complete or you cancel it.

当你提交一个 prompt 后，Codex 会以循环方式工作：它调用模型，然后执行模型输出所指示的操作，例如读取文件、编辑文件和发起工具调用。这一过程在任务完成或你取消操作时结束。

As with ChatGPT, Codex is only as effective as the instructions you give it. Here are some tips we find helpful when prompting Codex:

与 ChatGPT 一样，Codex 的效果取决于你给出的指令。以下是一些我们认为在编写 prompt 时有用的建议：

- Codex produces higher-quality outputs when it can verify its work. Include steps to reproduce an issue, validate a feature, and run linting and pre-commit checks.
- Codex handles complex work better when you break it into smaller, focused steps. Smaller tasks are easier for Codex to test and for you to review. If you're not sure how to split a task up, ask Codex to propose a plan.

- 当 Codex 能够验证自己的工作时，它会产出更高质量的输出。请提供复现问题的步骤、验证功能的方法，以及运行 lint 和 pre-commit 检查的命令。
- 当你将复杂工作拆分为更小、更聚焦的步骤时，Codex 处理起来效果更好。小任务既便于 Codex 测试，也便于你审查。如果你不确定如何拆分任务，可以请 Codex 提出一个计划。

### Thread model

A thread is a single session: your prompt plus the model outputs and tool calls that follow. A thread can include multiple prompts. For example, your first prompt might ask Codex to implement a feature, and a follow-up prompt might ask it to add tests.

thread（会话线程）是一个独立的会话：包含你的 prompt 以及随后的模型输出和工具调用。一个 thread 可以包含多个 prompt。例如，你的第一个 prompt 可能要求 Codex 实现某个功能，而后续的 prompt 可能要求它补充测试。

A thread is said to be "running" when Codex is actively working on it. You can run multiple threads at once, but avoid having two threads modify the same files. You can also resume a thread later by continuing it with another prompt.

当 Codex 正在处理某个 thread 时，该 thread 处于"运行中"状态。你可以同时运行多个 thread，但应避免两个 thread 修改同一批文件。你也可以之后通过发送新的 prompt 来恢复一个 thread。

Threads can run either:

Thread 可以在以下环境中运行：

- Cloud threads run in an isolated environment. Codex clones your repository and checks out the branch it's working on. Cloud threads are useful when you want to run work in parallel or delegate tasks from another device. To use cloud threads with your repo, push your code to GitHub first. You can also delegate tasks from your local machine, which includes your current working state.

- Cloud threads（云端线程）在隔离环境中运行。Codex 会克隆你的仓库并检出它正在工作的分支。当你需要并行运行任务或从其他设备委派任务时，cloud threads 非常有用。要在你的仓库中使用 cloud threads，请先将代码推送到 GitHub。你也可以从本地机器委派任务，这会包含你当前的工作状态。

In the Codex app, you can also start a chat without choosing a project. Chats aren't tied to a saved repository or project folder. Use them for research, planning, connected-tool workflows, or other work where Codex shouldn't start from a codebase. Chats use a Codex-managed `threads` directory under your Codex home as their working location. By default, that location is `~/.codex/threads`.

在 Codex 应用中，你还可以在不选择项目的情况下发起一个 chat（对话）。chat 不绑定到已保存的仓库或项目文件夹。将它们用于研究、规划、connected-tool workflow（连接工具的工作流），或其他不需要 Codex 从代码库开始的工作。chat 使用 Codex 主目录下由 Codex 管理的 `threads` 目录作为工作位置。默认情况下，该位置为 `~/.codex/threads`。

To change the base location for this state, set `CODEX_HOME`; see Config and state locations.

要更改此状态的根位置，请设置 `CODEX_HOME`；参见"Config and state locations"（配置与状态位置）文档。

When you submit a prompt, include context that Codex can use, such as references to relevant files and images. The Codex IDE extension automatically includes the list of open files and the selected text range as context.

当你提交 prompt 时，请附上 Codex 可以使用的 context（上下文），例如相关文件和图片的引用。Codex IDE 扩展会自动将当前打开的文件列表和选中的文本范围作为 context 包含在内。

As the agent works, it also gathers context from file contents, tool output, and an ongoing record of what it has done and what it still needs to do.

随着 agent（智能体）的工作推进，它还会从文件内容、工具输出，以及已完成工作和待办事项的持续记录中收集 context。

All information in a thread must fit within the model's context window, which varies by model. Codex monitors and reports the remaining space. For longer tasks, Codex may automatically compact the context by summarizing relevant information and discarding less relevant details. With repeated compaction, Codex can continue working on complex tasks over many steps.

thread 中的所有信息必须容纳在模型的 context window（上下文窗口）内，不同模型的窗口大小各异。Codex 会监控并报告剩余空间。对于较长的任务，Codex 可能会通过摘要相关信息和丢弃较不重要的细节来自动执行 compaction（上下文压缩）。通过反复 compaction，Codex 可以在多个步骤中持续处理复杂任务。

### Goal mode

Goal mode gives Codex a persistent objective to work toward across a longer task. Use it when the work may take many steps, or when Codex needs a clear definition of done that it can keep checking as it works.

Goal mode（目标模式）为 Codex 提供一个持久的目标，让它在较长的任务中持续推进。当工作可能需要多个步骤，或者 Codex 需要一个清晰的"完成"定义以便在工作中不断核验时，使用此模式。

When you set a goal, the goal text acts as both the starting prompt and the completion criteria. Codex uses it to decide what to do next and whether the task is complete. Start Goal mode with `/goal` in the Codex app, IDE extension, or CLI.

当你设定一个 goal（目标）后，goal 文本同时充当起始 prompt 和完成标准。Codex 用它来决定下一步做什么，以及任务是否已完成。在 Codex 应用、IDE 扩展或 CLI 中使用 `/goal` 启动 Goal mode。

Enable it by setting `features.goals` in `config.toml`. You can also run `codex features enable goals` from the CLI or ask Codex to run it.

通过在 `config.toml` 中设置 `features.goals` 来启用该功能。你也可以从 CLI 运行 `codex features enable goals`，或者让 Codex 来执行此命令。

In the Codex app, progress appears above the composer with controls to pause, resume, edit, or clear the goal.

在 Codex 应用中，进度会显示在输入框上方，并提供暂停、恢复、编辑或清除 goal 的控制按钮。

Write goals so Codex can tell whether it has succeeded. Good goals include a specific outcome, measurable target, or test criteria. For example:

编写 goal 时要确保 Codex 能够判断自己是否已成功完成。好的 goal 包含明确的结果、可衡量的目标或测试标准。例如：

```text
Migrate this codebase from JavaScript to TypeScript. The app should compile in
strict mode without explicit `any` type definitions.
```

```text
Reduce the time to interactive of the home page to below 1 second.
```

If the goal is hard to define up front, start with `/plan` and ask Codex to shape it before implementation. You can also ask Codex to interview you and draft a goal with clear success criteria.

如果 goal 一开始难以明确定义，可以用 `/plan` 开始，让 Codex 在实现之前先将其梳理成形。你也可以让 Codex 对你进行访谈，然后起草一个带有清晰成功标准的 goal。

You can continue steering Codex after the goal starts. Send follow-up messages to adjust constraints, such as asking Codex to use a particular library or avoid a specific approach. Use side chats when you want a status recap or explanation without interrupting the main task. For long-running work, pause the goal before you lose connectivity, then resume or edit it when you are ready to continue.

在 goal 开始后，你仍可以继续引导 Codex。发送后续消息来调整约束，例如要求 Codex 使用特定库或避免某种特定方法。当你需要状态回顾或解释、但不想打断主任务时，可以使用 side chat（侧边对话）。对于长时间运行的工作，请在失去网络连接前暂停 goal，然后在准备好继续时恢复或编辑它。

---

## Codex - Workflows（工作流）

原文链接：https://developers.openai.com/codex/workflows

### Introduction

Codex works best when you treat it like a teammate with explicit context and a clear definition of "done." This page gives end-to-end workflow (工作流) examples for the Codex IDE extension (IDE 插件), the Codex CLI (命令行工具), and Codex cloud (云端).

If you are new to Codex, read [Prompting](https://developers.openai.com/codex/prompting) first, then come back here for concrete recipes.

Codex 的最佳使用方式是把它当作一位需要明确 context（上下文）和清晰"完成标准"的队友。本页提供 Codex IDE 插件、Codex CLI 和 Codex 云端的端到端 workflow 示例。

如果你刚接触 Codex，建议先阅读 [Prompting](https://developers.openai.com/codex/prompting)，再回到本页查看具体实践方案。

Each workflow includes:

*   **When to use it** and which Codex surface fits best (IDE, CLI, or cloud).
*   **Steps** with example user prompts.
*   **Context notes**: what Codex automatically sees vs what you should attach.
*   **Verification**: how to check the output.

每个 workflow 包含：

*   **适用场景**，以及最适合的 Codex 界面（IDE、CLI 或云端）。
*   **操作步骤**，附带示例 prompt（提示词）。
*   **上下文说明**：Codex 能自动获取哪些信息，哪些需要你手动提供。
*   **验证方式**：如何检查输出结果。

> **Note:** The IDE extension automatically includes your open files as context. In the CLI, you usually need to mention paths explicitly (or attach files with `/mention` and `@` path autocomplete).

> **注意：** IDE 插件会自动将你打开的文件作为 context。在 CLI 中，通常需要显式指明路径（或通过 `/mention` 和 `@` 路径自动补全来附加文件）。

### Understand a codebase

Use this when you are onboarding, inheriting a service, or trying to reason about a protocol, data model, or request flow.

当你需要新人入职、接手某个服务，或想要梳理某个协议、数据模型或请求流程时，使用本 workflow。

#### IDE extension workflow (fastest for local exploration)

1.   Open the most relevant files.

2.   Select the code you care about (optional but recommended).

3.   Prompt Codex:

#### IDE 插件 workflow（本地探索最快）

1.  打开最相关的文件。

2.  选中你关注的代码（可选，但推荐）。

3.  向 Codex 发送 prompt：

```
Explain how the request flows through the selected code.

Include:
- a short summary of the responsibilities of each module involved
- what data is validated and where
- one or two "gotchas" to watch for when changing this
```

Verification:

*   Ask for a diagram or checklist you can validate quickly:

`Summarize the request flow as a numbered list of steps. Then list the files involved.`

验证：

*  让 Codex 给出一张图或一份清单，便于你快速核对：

`Summarize the request flow as a numbered list of steps. Then list the files involved.`

#### CLI workflow (good when you want a transcript + shell commands)

1.   Start an interactive session:

`codex`
2.   Attach the files (optional) and prompt:

`I need to understand the protocol used by this service. Read @foo.ts @schema.ts and explain the schema and request/response flow. Focus on required vs optional fields and backward compatibility rules.`

#### CLI workflow（需要会话记录 + shell 命令时适用）

1.  启动一个交互式会话：

`codex`
2.  附加文件（可选）并发送 prompt：

`I need to understand the protocol used by this service. Read @foo.ts @schema.ts and explain the schema and request/response flow. Focus on required vs optional fields and backward compatibility rules.`

Context notes:

*   You can use `@` in the composer to insert file paths from the workspace, or `/mention` to attach a specific file.

上下文说明：

*   你可以在输入框中使用 `@` 插入工作区中的文件路径，或用 `/mention` 附加指定文件。

### Fix a bug

Use this when you have a failing behavior you can reproduce locally.

当你有一个能在本地复现的故障行为时，使用本 workflow。

#### CLI workflow (tight loop with reproduction and verification)

1.   Start Codex at the repo root:

`codex`
2.   Give Codex a reproduction recipe, plus the file(s) you suspect:

#### CLI workflow（复现与验证的紧凑循环）

1.  在仓库根目录启动 Codex：

`codex`
2.  给 Codex 一个复现步骤，以及你怀疑的文件：

```
Bug: Clicking "Save" on the settings screen sometimes shows "Saved" but doesn't persist the change.

Repro:
1) Start the app: npm run dev
2) Go to /settings
3) Toggle "Enable alerts"
4) Click Save
5) Refresh the page: the toggle resets

Constraints:
- Do not change the API shape.
- Keep the fix minimal and add a regression test if feasible.

Start by reproducing the bug locally, then propose a patch and run checks.
```

Context notes:

*   Supplied by you: the repro steps and constraints (these matter more than a high-level description).
*   Supplied by Codex: command output, discovered call sites, and any stack traces it triggers.

上下文说明：

*   你提供的内容：复现步骤和约束条件（这些比高层面的描述更重要）。
*   Codex 提供的内容：命令输出、发现的调用点，以及它触发的任何 stack trace。

Verification:

*   Codex should re-run the repro steps after the fix.
*   If you have a standard check pipeline, ask it to run it:

`After the fix, run lint + the smallest relevant test suite. Report the commands and results.`

验证：

*   Codex 应在修复后重新执行复现步骤。
*   如果你有标准的检查流水线，让它运行：

`After the fix, run lint + the smallest relevant test suite. Report the commands and results.`

#### IDE extension workflow

1.   Open the file where you think the bug lives, plus its nearest caller.

2.   Prompt Codex:

`Find the bug causing "Saved" to show without persisting changes. After proposing the fix, tell me how to verify it in the UI.`

#### IDE 插件 workflow

1.  打开你认为存在 bug 的文件，以及离它最近的调用方。

2.  向 Codex 发送 prompt：

`Find the bug causing "Saved" to show without persisting changes. After proposing the fix, tell me how to verify it in the UI.`

### Write a test for a function

Use this when you want to be very explicit about the scope you want tested.

当你希望对要测试的范围非常明确时，使用本 workflow。

#### IDE extension workflow (selection-based)

1.   Open the file with the function.

2.   Select the lines that define the function. Choose "Add to Codex Thread" from command palette to add these lines to the context.

3.   Prompt Codex:

`Write a unit test for this function. Follow conventions used in other tests.`

#### IDE 插件 workflow（基于选区）

1.  打开包含该函数的文件。

2.  选中定义函数的代码行。从命令面板选择 "Add to Codex Thread"，将这些代码行加入 context。

3.  向 Codex 发送 prompt：

`Write a unit test for this function. Follow conventions used in other tests.`

Context notes:

*   Supplied by "Add to Codex Thread" command: the selected lines (this is the "line number" scope), plus open files.

上下文说明：

*   由 "Add to Codex Thread" 命令提供：选中的代码行（即"行号"范围），以及已打开的文件。

#### CLI workflow (path + line range described in prompt)

1.   Start Codex:

`codex`
2.   Prompt with a function name:

`Add a test for the invert_list function in @transform.ts. Cover the happy path plus edge cases.`

#### CLI workflow（在 prompt 中描述路径 + 行范围）

1.  启动 Codex：

`codex`
2.  用函数名发送 prompt：

`Add a test for the invert_list function in @transform.ts. Cover the happy path plus edge cases.`

### Build a UI from a mock or screenshot

Use this when you have a design mock, screenshot, or UI reference and you want a working prototype quickly.

当你有设计稿、截图或 UI 参考图，并希望快速得到一个可运行的原型时，使用本 workflow。

#### CLI workflow (image + prompt)

1.   Save your screenshot locally (for example `./specs/ui.png`).

2.   Run Codex:

`codex`
3.   Drag the image file into the terminal to attach it to the prompt.

4.   Follow up with constraints and structure:

#### CLI workflow（图片 + prompt）

1.  将截图保存到本地（例如 `./specs/ui.png`）。

2.  运行 Codex：

`codex`
3.  将图片文件拖入终端以附加到 prompt。

4.  补充约束条件和结构要求：

```
Create a new dashboard based on this image.

Constraints:
- Use react, vite, and tailwind. Write the code in typescript.
- Match spacing, typography, and layout as closely as possible.

Deliverables:
- A new route/page that renders the UI
- Any small components needed
- README.md with instructions to run it locally
```

Context notes:

*   The image provides visual requirements, but you still need to specify the implementation constraints (framework, routing, component style).
*   For best results, include any non-obvious behavior in text (hover states, validation rules, keyboard interactions).

上下文说明：

*   图片提供了视觉需求，但你仍需指明实现约束（框架、路由、组件风格）。
*   为获得最佳效果，请用文字补充任何非显而易见的行为（悬停状态、校验规则、键盘交互）。

Verification:

*   Ask Codex to run the dev server (if allowed) and tell you exactly where to look:

`Start the dev server and tell me the local URL/route to view the prototype.`

验证：

*   让 Codex 启动开发服务器（如果允许）并明确告诉你在哪里查看：

`Start the dev server and tell me the local URL/route to view the prototype.`

#### IDE extension workflow (image + existing files)

1.   Attach the image in the Codex chat (drag-and-drop or paste).

2.   Prompt Codex:

#### IDE 插件 workflow（图片 + 已有文件）

1.  在 Codex 聊天中附加图片（拖拽或粘贴）。

2.  向 Codex 发送 prompt：

```
Create a new settings page. Use the attached screenshot as the target UI.
Follow design and visual patterns from other files in this project.
```

### Iterate on styling and visuals

Use this when you want a tight "design → tweak → refresh → tweak" loop while Codex edits code.

当你希望在 Codex 编辑代码时保持紧凑的"设计 → 微调 → 刷新 → 微调"循环时，使用本 workflow。

#### CLI workflow (run Vite, then iterate with small prompts)

1.   Start Codex:

`codex`
2.   Start the dev server in a separate terminal window:

`npm run dev`
3.   Prompt Codex to make changes:

`Propose 2-3 styling improvements for the landing page.`
4.   Pick a direction and iterate with small, specific prompts:

#### CLI workflow（启动 Vite，然后用小 prompt 迭代）

1.  启动 Codex：

`codex`
2.  在另一个终端窗口启动开发服务器：

`npm run dev`
3.  让 Codex 提出修改：

`Propose 2-3 styling improvements for the landing page.`
4.  选定一个方向，用小而具体的 prompt 迭代：

```
Go with option 2.

Change only the header:
- make the typography more editorial
- increase whitespace
- ensure it still looks good on mobile
```
5.   Repeat with focused requests:

```
Next iteration: reduce visual noise.
Keep the layout, but simplify colors and remove any redundant borders.
```

5.  用聚焦的请求重复迭代：

```
Next iteration: reduce visual noise.
Keep the layout, but simplify colors and remove any redundant borders.
```

Verification:

*   Review changes in the browser "live" as the code is updated.
*   Commit changes that you like and revert those that you don't.
*   If you revert or modify a change, tell Codex so it doesn't overwrite the change when it works on the next prompt.

验证：

*   随着代码更新，在浏览器中"实时"查看变更。
*   提交你满意的改动，回退不满意的。
*   如果你回退或修改了某处改动，请告知 Codex，以免它在处理下一个 prompt 时覆盖该改动。

### Plan locally, then delegate to cloud

Use this when you want to design carefully (local context, quick inspection), then outsource the long implementation to a cloud task that can run in parallel.

当你希望在本地仔细设计（本地 context、快速检视），再把漫长的实现工作交给可并行运行的云端任务时，使用本 workflow。

#### Local planning (IDE)

1.   Make sure your current work is committed or at least stashed so you can compare changes cleanly.

2.   Ask Codex to produce a refactor plan. If you have the `$plan` skill (技能) available, invoke it explicitly:

#### 本地规划（IDE）

1.  确保当前工作已提交或至少已 stash，以便干净地对比变更。

2.  让 Codex 产出一份重构计划。如果你有 `$plan` skill，显式调用它：

```
$plan

We need to refactor the auth subsystem to:
- split responsibilities (token parsing vs session loading vs permissions)
- reduce circular imports
- improve testability

Constraints:
- No user-visible behavior changes
- Keep public APIs stable
- Include a step-by-step migration plan
```
3.   Review the plan and negotiate changes:

3.  审阅计划并协商调整：

```
Revise the plan to:
- specify exactly which files move in each milestone
- include a rollback strategy
```

Context notes:

*   Planning works best when Codex can scan the current code locally (entrypoints, module boundaries, dependency graph hints).

上下文说明：

*   当 Codex 能在本地扫描当前代码（入口、模块边界、依赖图线索）时，规划效果最佳。

#### Cloud delegation (IDE → Cloud)

1.   If you haven't already done so, set up a [Codex cloud environment](https://developers.openai.com/codex/cloud/environments).

2.   Click on the cloud icon beneath the prompt composer and select your cloud environment.

3.   When you enter the next prompt, Codex creates a new thread (会话线程) in the cloud that carries over the existing thread context (including the plan and any local source changes).

`Implement Milestone 1 from the plan.`
4.   Review the cloud diff, iterate if needed.

5.   Create a PR directly from the cloud or pull changes locally to test and finish up.

6.   Iterate on additional milestones of the plan.

#### 云端委派（IDE → 云端）

1.  如果尚未设置，先配置一个 [Codex 云端环境](https://developers.openai.com/codex/cloud/environments)。

2.  点击 prompt 输入框下方的云端图标，选择你的云端环境。

3.  当你输入下一条 prompt 时，Codex 会在云端创建一个新的 thread，并携带现有 thread 的 context（包括计划和任何本地源码改动）。

`Implement Milestone 1 from the plan.`
4.  审阅云端 diff，按需迭代。

5.  直接从云端创建 PR（pull request，拉取请求），或在本地拉取改动进行测试和收尾。

6.  继续迭代计划中的后续里程碑。

### Review your own changes (code review)

Use this when you want a second set of eyes before committing or creating a PR.

当你希望在提交或创建 PR 之前得到第二视角的审查时，使用本 workflow。

#### CLI workflow (review your working tree)

1.   Start Codex:

`codex`
2.   Run the review command:

`/review`
3.   Optional: provide custom focus instructions:

`/review Focus on edge cases and security issues`

#### CLI workflow（审查你的工作区）

1.  启动 Codex：

`codex`
2.  运行审查命令：

`/review`
3.  可选：提供自定义的关注指令：

`/review Focus on edge cases and security issues`

Verification:

*   Apply fixes based on review feedback, then rerun `/review` to confirm issues are resolved.

验证：

*   根据审查反馈应用修复，然后重新运行 `/review` 确认问题已解决。

### Review a pull request on GitHub

Use this when you want review feedback without pulling the branch locally.

当你希望在不把分支拉到本地的情况下获得审查反馈时，使用本 workflow。

Before you can use this, enable Codex **Code review** (代码审查) on your repository. See [Code review](https://developers.openai.com/codex/integrations/github).

使用前，需在你的仓库上启用 Codex 的 **Code review**。参见 [Code review](https://developers.openai.com/codex/integrations/github)。

1.   Open the pull request on GitHub.

2.   Leave a comment that tags Codex with explicit focus areas:

`@codex review`
3.   Optional: Provide more explicit instructions.

`@codex review for security vulnerabilities and security concerns`

1.  在 GitHub 上打开该 pull request。

2.  留下一条 @ Codex 的评论，指明关注重点：

`@codex review`
3.  可选：提供更明确的指令。

`@codex review for security vulnerabilities and security concerns`

### Update documentation

Use this when you need a doc change that is accurate and clear.

当你需要准确且清晰的文档变更时，使用本 workflow。

#### IDE or CLI workflow (local edits + local validation)

1.   Identify the doc file(s) to change and open them (IDE) or `@` mention them (IDE or CLI).

2.   Prompt Codex with scope and validation requirements:

`Update the "advanced features" documentation to provide authentication troubleshooting guidance. Verify that all links are valid.`
3.   After Codex drafts the changes, review the documentation and iterate as needed.

#### IDE 或 CLI workflow（本地编辑 + 本地验证）

1.  确定要修改的文档文件并打开它们（IDE），或用 `@` 提及它们（IDE 或 CLI）。

2.  向 Codex 发送包含范围和验证要求的 prompt：

`Update the "advanced features" documentation to provide authentication troubleshooting guidance. Verify that all links are valid.`
3.  Codex 起草变更后，审阅文档并按需迭代。

Verification:

*   Read the rendered page.

验证：

*   阅读渲染后的页面。

---

## Codex - Best practices（最佳实践）

原文链接：https://developers.openai.com/codex/learn/best-practices

If you're new to Codex or coding agents (编码代理，指能自主读取、编辑、运行代码的 AI 智能体) in general, this guide will help you get better results faster. It covers the core habits that make Codex more effective across the CLI, IDE extension, and the Codex app, from prompting and planning to validation, MCP, skills, and automations.

如果你刚接触 Codex 或编码代理，本指南将帮助你更快地获得更好的效果。内容涵盖让 Codex 在 CLI、IDE 扩展和 Codex app 中更高效运作的核心习惯，从提示词与规划，到验证、MCP、skills（技能，可复用的工作流封装）以及 automations（自动化，按计划重复执行的任务）。

Codex works best when you treat it less like a one-off assistant and more like a teammate you configure and improve over time.

Codex 的最佳使用方式，是不要把它当成一次性的助手，而是当作一个你持续配置、不断改进的队友。

A useful way to think about this: start with the right task context, use `AGENTS.md` for durable guidance, configure Codex to match your workflow, connect external systems with MCP, turn repeated work into skills, and automate stable workflows.

可以这样理解整体思路：以正确的任务上下文起步，用 `AGENTS.md` 沉淀持久化的指导，配置 Codex 以匹配你的工作流，用 MCP 连接外部系统，把重复性工作封装成 skills，并将稳定的工作流自动化。

### Strong first use: Context and prompts

Codex is already strong enough to be useful even when your prompt isn't perfect. You can often hand it a hard problem with minimal setup and still get a strong result. Clear prompting isn't required to get value, but it does make results more reliable, especially in larger codebases or higher-stakes tasks.

### 首次使用的关键：上下文与提示词

即使你的提示词不够完美，Codex 也已经足够强大，能够产生有用的结果。你常常只需最少量的准备，就能把一个难题交给它并得到不错的输出。清晰的提示词并非获得价值的必要条件，但它确实能让结果更可靠——尤其是在大型代码库或高风险任务中。

If you work in a large or complex repository, the biggest unlock is giving Codex the right task context and a clear structure for what you want done.

如果你在大型或复杂的代码仓库中工作，最大的提升杠杆是给 Codex 正确的任务上下文，以及对你期望成果的清晰结构化描述。

A good default is to include four things in your prompt:

一个不错的默认做法是在提示词中包含四项内容：

- Goal: What are you trying to change or build?
- Context: Which files, folders, docs, examples, or errors matter for this task? You can @ mention certain files as context.
- Constraints: What standards, architecture, safety requirements, or conventions should Codex follow?
- Done when: What should be true before the task is complete, such as tests passing, behavior changing, or a bug no longer reproducing?

- Goal（目标）：你想要改动或构建什么？
- Context（上下文）：哪些文件、目录、文档、示例或报错与本任务相关？你可以用 @ 提及某些文件作为上下文。
- Constraints（约束）：Codex 应遵循哪些标准、架构、安全要求或约定？
- Done when（完成条件）：任务完成前应满足什么条件，例如测试通过、行为已改变、或某个 bug 不再复现？

This helps Codex stay scoped, make fewer assumptions, and produce work that's easier to review.

这能帮助 Codex 保持范围聚焦、减少臆测，并产出更易于审查的成果。

### Plan first for difficult tasks

If the task is complex, ambiguous, or hard to describe well, ask Codex to plan before it starts coding.

### 复杂任务先做规划

如果任务复杂、模糊或难以清晰描述，就让 Codex 在动手写代码之前先做规划。

### Make guidance reusable with `AGENTS.md`

Think of `AGENTS.md` as an open-format README for agents. It loads into context automatically and is the best place to encode how you and your team want Codex to work in a repository.

### 用 `AGENTS.md` 沉淀可复用的指导

把 `AGENTS.md` 想象成给 agent（智能体）看的开放式 README。它会自动加载到上下文中，是记录你和团队希望 Codex 在某个仓库中如何运作的最佳位置。

A good `AGENTS.md` covers:

一个合格的 `AGENTS.md` 应包含：

- repo layout and important directories
- How to run the project
- Build, test, and lint commands
- Engineering conventions and PR expectations
- Constraints and do-not rules
- What done means and how to verify work

- 仓库布局与重要目录
- 如何运行项目
- 构建、测试和 lint 命令
- 工程约定与 PR（pull request，代码合并请求）期望
- 约束与禁止规则
- "完成"的含义以及如何验证成果

The `/init` slash command in the CLI is the quick-start command to scaffold a starter `AGENTS.md` in the current directory. It's a great starting point, but you should edit the result to match how your team actually builds, tests, reviews, and ships code.

CLI 中的 `/init` 斜杠命令是快速上手命令，用于在当前目录生成一个 `AGENTS.md` 脚手架。这是一个很好的起点，但你应当编辑其结果，使其与你团队实际构建、测试、审查和发布代码的方式保持一致。

Keep it practical. A short, accurate `AGENTS.md` is more useful than a long file full of vague rules. Start with the basics, then add new rules only after you notice repeated mistakes.

保持实用。一份简短且准确的 `AGENTS.md` 比一份充满模糊规则的长文件更有用。从基础内容开始，只有在你发现重复出现的错误后才添加新规则。

### Configure Codex for consistency

Configuration is one of the main ways to make Codex behave more consistently across sessions and surfaces. For example, you can set defaults for model choice, reasoning effort, sandbox mode, approval policy, profiles, and MCP setup.

### 配置 Codex 以保持一致性

配置是让 Codex 在不同会话和不同入口间行为更一致的主要手段之一。例如，你可以为模型选择、推理投入程度、sandbox（沙盒，限制 agent 读写权限的隔离环境）模式、审批策略、profiles（配置档）以及 MCP 设置设定默认值。

- Keep personal defaults in `~/.codex/config.toml` (Settings → Configuration → Open config.toml from the Codex app)
- Keep repo-specific behavior in `.codex/config.toml`
- Use command-line overrides only for one-off situations (if you use the CLI)

- 个人默认配置放在 `~/.codex/config.toml`（在 Codex app 中通过 Settings → Configuration → Open config.toml 打开）
- 仓库专属行为放在 `.codex/config.toml`
- 命令行覆盖参数仅用于一次性场景（如果你使用 CLI）

Profile-specific overrides live in separate `$CODEX_HOME/profile-name.config.toml` files.

针对特定 profile 的覆盖配置存放在独立的 `$CODEX_HOME/profile-name.config.toml` 文件中。

### Improve reliability with testing and review

Don't stop at asking Codex to make a change. Ask it to create tests when needed, run the relevant checks, confirm the result, and review the work before you accept it.

### 通过测试与审查提升可靠性

不要只让 Codex 做完改动就结束。需要时让它创建测试、运行相关检查、确认结果，并在你接受之前先审查它的工作。

If you're new to coding agents, start with the default permissions. Keep approval and sandboxing tight by default, then loosen permissions only for trusted repos or specific workflows once the need is clear.

如果你刚开始使用编码代理，请从默认权限开始。默认情况下保持审批与沙盒收紧，只有在需求明确后，才对受信任的仓库或特定工作流放宽权限。

### Use MCPs for external context

Use MCPs when the context Codex needs lives outside the repo. It lets Codex connect to the tools and systems you already use, so you don't have to keep copying and pasting live information into prompts.

### 用 MCP 获取外部上下文

当 Codex 所需的上下文存在于仓库之外时，就使用 MCP。MCP（Model Context Protocol，模型上下文协议）让 Codex 连接到你已在使用的工具和系统，这样你就不必反复把实时信息复制粘贴到提示词中。

Use MCP when:

在以下情况使用 MCP：

- The needed context lives outside the repo
- The data changes frequently
- You want Codex to use a tool rather than rely on pasted instructions
- You need a repeatable integration across users or projects

- 所需上下文存在于仓库之外
- 数据频繁变化
- 你希望 Codex 调用某个工具，而非依赖粘贴进来的说明
- 你需要在多个用户或项目间复用同一套集成

You can ask Codex to set up an MCP server in plain language. All you need to do is ask. You can also use the `codex mcp add` command in the CLI to add your custom servers with a name, URL, and other details.

你可以用自然语言让 Codex 配置一个 MCP 服务器，只需开口即可。你也可以在 CLI 中使用 `codex mcp add` 命令添加自定义服务器，指定名称、URL 及其他细节。

Add tools only when they unlock a real workflow. Do not start by wiring in every tool you use. Start with one or two tools that clearly remove a manual loop you already do often, then expand from there.

只在实际能解锁某个工作流时才添加工具。不要一上来就把你用的每个工具都接入。先从一两个能明显消除你常做的某个手动循环的工具开始，之后再逐步扩展。

### Use automations for repeated work

Once a workflow becomes repeatable, stop relying on long prompts or repeated back-and-forth. Use a Skill to package the instructions in a `SKILL.md` file, context, and supporting logic Codex should apply consistently. Skills work across the CLI, IDE extension, and Codex app.

### 用自动化处理重复性工作

当某个工作流变得可重复时，就别再依赖冗长的提示词或反复来回沟通。使用 Skill 把指令、上下文以及 Codex 应一致执行的辅助逻辑打包进一个 `SKILL.md` 文件。Skills 可跨 CLI、IDE 扩展和 Codex app 使用。

Once a workflow is stable, you can schedule Codex to run it in the background for you. In the Codex app, automations let you choose the project, prompt, cadence, and execution environment for a recurring task.

当某个工作流稳定后，你可以安排 Codex 在后台为你运行它。在 Codex app 中，automations 允许你为周期性任务选择项目、提示词、执行频率和执行环境。

You can also choose whether the automation runs in a dedicated git worktree or in your local environment.

你还可以选择该自动化是在专用的 git worktree（工作树，同一仓库的独立工作目录分支）中运行，还是在你的本地环境中运行。

A useful rule is that skills define the method, automations define the schedule. If a workflow still needs a lot of steering, turn it into a skill first. Once it's predictable, automation becomes a force multiplier.

一条有用的规则是：skills 定义方法，automations 定义时间。如果某个工作流仍需要大量人为引导，先把它做成 skill。等它变得可预测后，自动化就会成为力量的倍增器。

Use automations for reflection and maintenance, not just execution. Review recent sessions, summarize repeated friction, and improve prompts, instructions, or workflow setup over time.

把自动化用于反思与维护，而不仅仅是执行。审查近期的会话、总结反复出现的摩擦点，并随时间推移改进提示词、指令或工作流配置。

### Common mistakes

A few common mistakes to avoid when first using Codex:

### 常见错误

初次使用 Codex 时要避免的几个常见错误：

- Overloading the prompt with durable rules instead of moving them into `AGENTS.md` or a skill
- Not letting the agent see its work by not giving details on how to best run build and test commands
- Skipping planning on multi-step and complex tasks
- Giving Codex full permission to your computer before you understand the workflow
- Running live threads on the same files without using git worktrees
- Turning a recurring task into an automation before it's reliable manually
- Treating Codex like something you have to watch step by step instead of using it in parallel with your own work
- Using one thread per project instead of one thread per task. This leads to bloated context and worse results over time

- 把持久化规则塞进提示词，而不是移入 `AGENTS.md` 或某个 skill
- 没有告诉 agent 如何最好地运行构建和测试命令，导致它无法看到自己的工作成果
- 在多步骤和复杂任务上跳过规划
- 在还没理解工作流之前就给 Codex 对你电脑的完整权限
- 在同一批文件上同时跑多个活跃线程，却不使用 git worktree
- 在某个周期性任务还无法手动可靠完成之前就把它变成自动化
- 把 Codex 当成必须逐步盯着的工具，而不是与自己的工作并行使用
- 每个项目只用一个线程，而不是每个任务一个线程。这会导致上下文膨胀，长期下来效果变差

---

## Codex - Custom instructions with AGENTS.md（用 AGENTS.md 自定义指令）

原文链接：https://developers.openai.com/codex/guides/agents-md

### Custom instructions with AGENTS.md

Codex reads `AGENTS.md` files before doing any work. By layering global guidance with project-specific overrides, you can start each task with consistent expectations, no matter which repository you open.

Codex 会在执行任何工作之前读取 `AGENTS.md` 文件。通过将全局指导（global guidance，适用于所有项目的通用规则）与项目级覆盖（project-specific overrides，针对特定项目的定制规则）分层叠加，无论你打开哪个代码仓库，都能让每个任务在一致的预期下开始。

### How discovery works

Codex builds an instruction chain when it starts (once per run; in the TUI this usually means once per launched session). Discovery follows this precedence order:

Codex 在启动时会构建一条指令链（每次运行构建一次；在 TUI 终端界面中通常指每次启动会话时构建一次）。发现过程遵循以下优先级顺序：

1. Global scope: In your Codex home directory (defaults to `~/.codex`, unless you set `CODEX_HOME`), Codex reads `AGENTS.override.md` if it exists. Otherwise, Codex reads `AGENTS.md`. Codex uses only the first non-empty file at this level.

1. 全局作用域（Global scope）：在你的 Codex 主目录中（默认为 `~/.codex`，除非你设置了 `CODEX_HOME`），如果 `AGENTS.override.md` 存在，Codex 会优先读取它；否则读取 `AGENTS.md`。在此层级，Codex 只使用第一个非空文件。

2. Project scope: Starting at the project root (typically the Git root), Codex walks down to your current working directory. If Codex cannot find a project root, it only checks the current directory. In each directory along the path, it checks for `AGENTS.override.md`, then `AGENTS.md`, then any fallback names in `project_doc_fallback_filenames`. Codex includes at most one file per directory.

2. 项目作用域（Project scope）：从项目根目录（通常是 Git 根目录）开始，Codex 会沿路径向下逐层遍历直到你的当前工作目录。如果找不到项目根目录，则只检查当前目录。在路径上的每个目录中，Codex 依次查找 `AGENTS.override.md`，然后是 `AGENTS.md`，再然后是 `project_doc_fallback_filenames` 中配置的任何回退文件名。每个目录最多包含一个文件。

3. Merge order: Codex concatenates files from the root down, joining them with blank lines. Files closer to your current directory override earlier guidance because they appear later in the combined prompt.

3. 合并顺序（Merge order）：Codex 从根目录向下拼接文件，文件之间用空行分隔。离当前目录越近的文件出现在合并后提示词的越靠后位置，因此会覆盖更早的指导内容。

Codex skips empty files and stops adding files once the combined size reaches the limit defined by `project_doc_max_bytes` (32 KiB by default). For details on these knobs, see Project instructions discovery. Raise the limit or split instructions across nested directories when you hit the cap.

Codex 会跳过空文件，并在合并后的大小达到 `project_doc_max_bytes` 定义的上限（默认 32 KiB）时停止添加文件。有关这些配置项的详细信息，请参阅 Project instructions discovery。当达到上限时，可以调高限制或将指令拆分到嵌套的子目录中。

Use `~/.codex/AGENTS.override.md` when you need a temporary global override without deleting the base file. Remove the override to restore the shared guidance.

当你需要临时进行全局覆盖而又不想删除基础文件时，可以使用 `~/.codex/AGENTS.override.md`。删除该覆盖文件即可恢复共享的指导内容。

### Layer project instructions

Repository-level files keep Codex aware of project norms while still inheriting your global defaults.

仓库级文件能让 Codex 了解项目规范，同时仍然继承你的全局默认设置。

In your repository root, add an `AGENTS.md` that covers basic setup:

在你的仓库根目录下，添加一个 `AGENTS.md` 来描述基本配置：

```markdown
# My project

## Build & test

- Run `pnpm test` to execute the test suite.
- Run `pnpm lint` before opening a pull request.

## Conventions

- Use named exports; avoid default exports.
- All API endpoints require authentication middleware.
```

Add overrides in nested directories when specific teams need different rules. For example, inside `services/payments/` create `AGENTS.override.md`:

当特定团队需要不同的规则时，可在嵌套目录中添加覆盖文件。例如，在 `services/payments/` 目录下创建 `AGENTS.override.md`：

```markdown
# Payments service

## Testing

- Integration tests require a running PostgreSQL container.
- Run `docker compose up -d db` before running the test suite.
- Use `make test-payments` instead of the project-level test command.
```

Expected: Codex reports the global file first, the repository root `AGENTS.md` second, and the payments override last.

预期结果：Codex 会先报告全局文件，其次是仓库根目录的 `AGENTS.md`，最后是 payments 的覆盖文件。

Codex stops searching once it reaches your current directory, so place overrides as close to specialized work as possible.

Codex 一旦到达当前目录就会停止搜索，因此请将覆盖文件放置在尽可能靠近专门工作位置的地方。

### Customize fallback filenames

If your repository already uses a different filename (for example `TEAM_GUIDE.md`), add it to the fallback list so Codex treats it like an instructions file.

如果你的仓库已经使用了其他文件名（例如 `TEAM_GUIDE.md`），可以将其添加到回退列表中，让 Codex 将其视为指令文件。

```toml
# ~/.codex/config.toml
project_doc_fallback_filenames = ["TEAM_GUIDE.md", ".agents.md"]
project_doc_max_bytes = 65536
```

Now Codex checks each directory in this order: `AGENTS.override.md`, `AGENTS.md`, `TEAM_GUIDE.md`, `.agents.md`. Filenames not on this list are ignored for instruction discovery. The larger byte limit allows more combined guidance before truncation.

现在 Codex 会按以下顺序检查每个目录：`AGENTS.override.md`、`AGENTS.md`、`TEAM_GUIDE.md`、`.agents.md`。不在此列表中的文件名在指令发现过程中会被忽略。更大的字节上限允许在截断前容纳更多合并后的指导内容。

### Verify your setup

- Run `codex --ask-for-approval never "Summarize the current instructions."` from a repository root. Codex should echo guidance from global and project files in precedence order.

- 在仓库根目录下运行 `codex --ask-for-approval never "Summarize the current instructions."`。Codex 应按照优先级顺序回显来自全局文件和项目文件的指导内容。

- Use `codex --cd subdir --ask-for-approval never "Show which instruction files are active."` to confirm nested overrides replace broader rules.

- 使用 `codex --cd subdir --ask-for-approval never "Show which instruction files are active."` 来确认嵌套覆盖是否替换了更宽泛的规则。

- To audit which instruction files Codex loaded, opt into a plaintext TUI log with `codex -c log_dir=./.codex-log` and check `./.codex-log/codex-tui.log`, or inspect the most recent `session-*.jsonl` file if you enabled session logging.

- 若要审计 Codex 加载了哪些指令文件，可以通过 `codex -c log_dir=./.codex-log` 开启纯文本 TUI 日志，然后检查 `./.codex-log/codex-tui.log`；如果启用了会话日志，也可以查看最新的 `session-*.jsonl` 文件。

- If instructions look stale, restart Codex in the target directory. Codex rebuilds the instruction chain on every run (and at the start of each TUI session), so there is no cache to clear manually.

- 如果指令看起来已过时，请在目标目录中重启 Codex。Codex 在每次运行时（以及每个 TUI 会话开始时）都会重新构建指令链，因此无需手动清除缓存。

### Troubleshooting

- Codex ignores fallback names: Confirm you listed the names in `project_doc_fallback_filenames` without typos, then restart Codex so the change takes effect.

- Codex 忽略回退文件名：确认你在 `project_doc_fallback_filenames` 中列出的文件名没有拼写错误，然后重启 Codex 使更改生效。

- Wrong guidance appears: Look for an `AGENTS.override.md` higher in the directory tree or under your Codex home. Rename or remove the override to fall back to the regular file.

- 出现错误的指导内容：检查目录树更高层级或 Codex 主目录下是否存在 `AGENTS.override.md`。重命名或删除该覆盖文件即可回退到常规文件。

- Instructions truncated: Raise `project_doc_max_bytes` or split large files across nested directories to keep critical guidance intact.

- 指令被截断：调高 `project_doc_max_bytes` 的值，或将大文件拆分到嵌套子目录中，以确保关键指导内容完整保留。

---

## Codex - Agent Skills（智能体技能）

原文链接：https://developers.openai.com/codex/skills

### Agent Skills

Use agent skills to extend Codex with task-specific capabilities. A skill packages instructions, resources, and optional scripts so Codex can follow a consistent process for tasks you repeat.

使用 agent skills（智能体技能）来扩展 Codex 的任务特定能力。一个 skill 将指令、资源和可选脚本打包在一起，使 Codex 能够为你重复执行的任务遵循一致的流程。

Skills are modular, self-contained folders that extend Codex's capabilities by providing specialized knowledge, workflows, and tools. Think of them as "onboarding guides" for specific domains or tasks—they transform Codex from a general-purpose agent into a specialized agent equipped with procedural knowledge that no model can fully possess.

Skills 是模块化、自包含的文件夹，通过提供专业知识、工作流和工具来扩展 Codex 的能力。可以把它们看作特定领域或任务的"入职指南"——它们将 Codex 从通用 agent（智能体）转变为配备了程序性知识的专业 agent，而这些知识是任何模型都无法完全内建的。

### What Skills Provide

1. Specialized workflows - Multi-step procedures for specific domains
2. Tool integrations - Instructions for working with specific file formats or APIs
3. Domain expertise - Company-specific knowledge, schemas, business logic
4. Bundled resources - Scripts, references, and assets for complex and repetitive tasks

### Skills 提供的能力

1. 专业化工作流——针对特定领域的多步骤流程
2. 工具集成——与特定文件格式或 API 协作的指令
3. 领域专业知识——公司特定的知识、数据模式、业务逻辑
4. 捆绑资源——用于复杂和重复任务的脚本、参考文档和素材

### Progressive Disclosure

Skills use progressive disclosure to manage context efficiently: Codex starts with each skill's name, description, and file path. Codex loads the full `SKILL.md` instructions only when it decides to use a skill.

### 渐进式加载

Skills 使用 progressive disclosure（渐进式加载）来高效管理上下文：Codex 启动时仅读取每个 skill 的名称、描述和文件路径。只有当 Codex 决定使用某个 skill 时，才会加载完整的 `SKILL.md` 指令。

Skills use a three-level loading system to manage context efficiently:

1. **Metadata (name + description)** - Always in context (~100 words)
2. **SKILL.md body** - When skill triggers (<5k words)
3. **Bundled resources** - As needed by Codex (unlimited, because scripts can be executed without reading into context window)

Skills 使用三级加载系统来高效管理上下文：

1. **元数据（name + description）**——始终在上下文中（约 100 词）
2. **SKILL.md 正文**——当 skill 被触发时加载（建议 <5k 词）
3. **捆绑资源**——按需由 Codex 加载（无限制，因为脚本可以直接执行而无需读入上下文窗口）

### Skill Structure

A skill is a directory with a `SKILL.md` file plus optional scripts and references. The `SKILL.md` file must include `name` and `description`.

### Skill 结构

一个 skill 是一个包含 `SKILL.md` 文件以及可选脚本和参考文档的目录。`SKILL.md` 文件必须包含 `name` 和 `description`。

Every skill consists of a required `SKILL.md` file and optional bundled resources:

```
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

每个 skill 由一个必需的 `SKILL.md` 文件和可选的捆绑资源组成：

```
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

Every `SKILL.md` consists of:

- **Frontmatter** (YAML): Contains `name` and `description` fields. These are the only fields that Codex reads to determine when the skill gets used, thus it is very important to be clear and comprehensive in describing what the skill is, and when it should be used.
- **Body** (Markdown): Instructions and guidance for using the skill. Only loaded AFTER the skill triggers (if at all).

每个 `SKILL.md` 包含：

- **Frontmatter**（YAML 前置元数据）：包含 `name` 和 `description` 字段。这是 Codex 用来判断何时使用该 skill 的唯一依据，因此清晰、全面地描述 skill 是什么以及何时使用它非常重要。
- **正文**（Markdown）：使用该 skill 及其捆绑资源的指令和指引。仅在 skill 被触发后才加载（如果会触发的话）。

#### Scripts (`scripts/`)

Executable code (Python/Bash/etc.) for tasks that require deterministic reliability or are repeatedly rewritten.

- **When to include**: When the same code is being rewritten repeatedly or deterministic reliability is needed
- **Example**: `scripts/rotate_pdf.py` for PDF rotation tasks
- **Benefits**: Token efficient, deterministic, may be executed without loading into context

#### 脚本（`scripts/`）

用于需要确定性可靠性或被反复重写的任务的可执行代码（Python/Bash 等）。

- **何时包含**：当同一段代码被反复重写，或需要确定性可靠性时
- **示例**：`scripts/rotate_pdf.py` 用于 PDF 旋转任务
- **优点**：Token 高效、确定性、可直接执行而无需加载到上下文中

#### References (`references/`)

Documentation and reference material intended to be loaded as needed into context to inform Codex's process and thinking.

- **When to include**: For documentation that Codex should reference while working
- **Examples**: `references/finance.md` for financial schemas, `references/api_docs.md` for API specifications
- **Benefits**: Keeps `SKILL.md` lean, loaded only when Codex determines it's needed
- **Best practice**: If files are large (>10k words), include grep search patterns in `SKILL.md`

#### 参考文档（`references/`）

旨在按需加载到上下文中以指导 Codex 流程和思考的文档及参考资料。

- **何时包含**：当有 Codex 在工作时应当参考的文档时
- **示例**：`references/finance.md` 用于财务数据模式，`references/api_docs.md` 用于 API 规范
- **优点**：保持 `SKILL.md` 精简，仅在 Codex 判断需要时才加载
- **最佳实践**：如果文件较大（>10k 词），在 `SKILL.md` 中提供 grep 搜索模式

#### Assets (`assets/`)

Files not intended to be loaded into context, but rather used within the output Codex produces.

- **When to include**: When the skill needs files that will be used in the final output
- **Examples**: `assets/logo.png` for brand assets, `assets/slides.pptx` for PowerPoint templates
- **Benefits**: Separates output resources from documentation, enables Codex to use files without loading them into context

#### 素材（`assets/`）

不打算加载到上下文中、而是用于 Codex 生成输出的文件。

- **何时包含**：当 skill 需要在最终输出中使用的文件时
- **示例**：`assets/logo.png` 用于品牌素材，`assets/slides.pptx` 用于 PowerPoint 模板
- **优点**：将输出资源与文档分离，使 Codex 无需将文件加载到上下文即可使用

### Using Skills

Codex can activate a skill in two ways:

1. **Explicit invocation**: Include the skill directly in your prompt. In CLI/IDE, run `/skills` or type `$` to mention a skill.
2. **Implicit invocation**: Codex can choose a skill when your task matches the skill `description`.

### 使用 Skills

Codex 可以通过两种方式激活 skill：

1. **显式调用**：在 prompt（提示词）中直接包含 skill。在 CLI/IDE 中，运行 `/skills` 或输入 `$` 来引用一个 skill。
2. **隐式调用**：当你的任务匹配 skill 的 `description` 时，Codex 可以自动选择该 skill。

Clear skill descriptions improve triggering reliability. Codex can still trigger a skill if descriptions are shortened.

清晰的 skill 描述能提高触发的可靠性。即使描述被缩短，Codex 仍然可以触发 skill。

### Create a Skill

You can create a skill using the built-in skill creator (`$skill-creator`), which asks what the skill does, when it should trigger, and whether it's instruction-only or script-backed (instruction-only is the default recommendation).

### 创建 Skill

你可以使用内置的 skill 创建器（`$skill-creator`）来创建 skill，它会询问 skill 的功能、触发时机，以及是纯指令型还是脚本型（默认推荐纯指令型）。

You can also create a skill manually by creating a folder with a `SKILL.md` file:

```md
---
name: skill-name
description: Explain exactly when this skill should and should not trigger.
---

Skill instructions for Codex to follow.
```

你也可以手动创建 skill，只需创建一个包含 `SKILL.md` 文件的文件夹：

```md
---
name: skill-name
description: Explain exactly when this skill should and should not trigger.
---

Skill instructions for Codex to follow.
```

The `description` is the primary triggering mechanism for your skill, and helps Codex understand when to use the skill. Include both what the skill does and specific triggers/contexts for when to use it. Include all "when to use" information here—not in the body. The body is only loaded after triggering, so "When to Use This Skill" sections in the body are not helpful to Codex.

`description` 是 skill 的主要触发机制，帮助 Codex 理解何时使用该 skill。需要同时包含 skill 的功能和具体的触发条件/场景。将所有"何时使用"的信息放在这里——而不是正文中。正文仅在触发后才加载，因此正文中的"何时使用此 Skill"章节对 Codex 没有帮助。

Example description for a `docx` skill: "Comprehensive document creation, editing, and analysis with support for tracked changes, comments, formatting preservation, and text extraction. Use when Codex needs to work with professional documents (.docx files) for: (1) Creating new documents, (2) Modifying or editing content, (3) Working with tracked changes, (4) Adding comments, or any other document tasks"

`docx` skill 的描述示例："Comprehensive document creation, editing, and analysis with support for tracked changes, comments, formatting preservation, and text extraction. Use when Codex needs to work with professional documents (.docx files) for: (1) Creating new documents, (2) Modifying or editing content, (3) Working with tracked changes, (4) Adding comments, or any other document tasks"

#### Initializing the Skill

When creating a new skill from scratch, always run the `init_skill.py` script. The script generates a new template skill directory that automatically includes everything a skill requires.

Usage:

```bash
scripts/init_skill.py <skill-name> --path <output-directory> [--resources scripts,references,assets] [--examples]
```

Examples:

```bash
scripts/init_skill.py my-skill --path "${CODEX_HOME:-$HOME/.codex}/skills"
scripts/init_skill.py my-skill --path "${CODEX_HOME:-$HOME/.codex}/skills" --resources scripts,references
scripts/init_skill.py my-skill --path ~/work/skills --resources scripts --examples
```

#### 初始化 Skill

从头创建新 skill 时，始终运行 `init_skill.py` 脚本。该脚本会生成一个包含 skill 所需全部内容的新模板 skill 目录。

用法：

```bash
scripts/init_skill.py <skill-name> --path <output-directory> [--resources scripts,references,assets] [--examples]
```

示例：

```bash
scripts/init_skill.py my-skill --path "${CODEX_HOME:-$HOME/.codex}/skills"
scripts/init_skill.py my-skill --path "${CODEX_HOME:-$HOME/.codex}/skills" --resources scripts,references
scripts/init_skill.py my-skill --path ~/work/skills --resources scripts --examples
```

The script:

- Creates the skill directory at the specified path
- Generates a `SKILL.md` template with proper frontmatter and TODO placeholders
- Creates `agents/openai.yaml` using agent-generated `display_name`, `short_description`, and `default_prompt` passed via `--interface key=value`
- Optionally creates resource directories based on `--resources`
- Optionally adds example files when `--examples` is set

该脚本会：

- 在指定路径创建 skill 目录
- 生成带有正确 frontmatter 和 TODO 占位符的 `SKILL.md` 模板
- 使用通过 `--interface key=value` 传入的 agent 生成的 `display_name`、`short_description` 和 `default_prompt` 创建 `agents/openai.yaml`
- 根据 `--resources` 可选地创建资源目录
- 当设置 `--examples` 时可选地添加示例文件

#### Validating the Skill

Once development of the skill is complete, validate the skill folder to catch basic issues early:

```bash
scripts/quick_validate.py <path/to/skill-folder>
```

The validation script checks YAML frontmatter format, required fields, and naming rules. If validation fails, fix the reported issues and run the command again.

#### 验证 Skill

skill 开发完成后，验证 skill 文件夹以尽早发现基本问题：

```bash
scripts/quick_validate.py <path/to/skill-folder>
```

验证脚本会检查 YAML frontmatter 格式、必填字段和命名规则。如果验证失败，修复报告的问题后重新运行该命令。

### Where Codex Reads Skills From

Codex reads skills from repository, user, admin, and system locations. For repositories, Codex scans `.agents/skills` in every directory from your current working directory up to the repository root. If two skills share the same `name`, Codex doesn't merge them; both can appear in skill selectors.

### Codex 从哪里读取 Skills

Codex 从仓库、用户、管理员和系统位置读取 skills。对于仓库，Codex 会从当前工作目录向上一直到仓库根目录，扫描每一级目录中的 `.agents/skills`。如果两个 skill 的 `name` 相同，Codex 不会合并它们；两者都可以出现在 skill 选择器中。

| Skill Scope | Location | Suggested use |
| --- | --- | --- |
| `REPO` | `$CWD/.agents/skills` | Current working directory: where you launch Codex. If you're in a repository or code environment, teams can check in skills relevant to a working folder. For example, skills only relevant to a microservice or a module. |
| `REPO` | `$CWD/../.agents/skills` | A folder above CWD when you launch Codex inside a Git repository. If you're in a repository with nested folders, organizations can check in skills relevant to a shared area in a parent folder. |
| `REPO` | `$REPO_ROOT/.agents/skills` | The topmost root folder when you launch Codex inside a Git repository. If you're in a repository with nested folders, organizations can check in skills relevant to everyone using the repository. These serve as root skills available to any subfolder in the repository. |
| `USER` | `$HOME/.agents/skills` | Any skills checked into the user's personal folder. Use to curate skills relevant to a user that apply to any repository the user may work in. |

| Skill 范围 | 位置 | 建议用途 |
| --- | --- | --- |
| `REPO` | `$CWD/.agents/skills` | 当前工作目录：你启动 Codex 的位置。如果你在仓库或代码环境中，团队可以签入与工作文件夹相关的 skills。例如，仅与某个微服务或模块相关的 skills。 |
| `REPO` | `$CWD/../.agents/skills` | 当你在 Git 仓库内启动 Codex 时，CWD 上方的文件夹。如果你在具有嵌套文件夹的仓库中，组织可以签入与父文件夹中共享区域相关的 skills。 |
| `REPO` | `$REPO_ROOT/.agents/skills` | 当你在 Git 仓库内启动 Codex 时的最顶层根文件夹。如果你在具有嵌套文件夹的仓库中，组织可以签入与所有使用该仓库的人相关的 skills。这些作为仓库中任何子文件夹都可用的根 skills。 |
| `USER` | `$HOME/.agents/skills` | 签入到用户个人文件夹的任何 skills。用于策划与用户相关的、适用于该用户可能工作的任何仓库的 skills。 |

### Agents Metadata

Add `agents/openai.yaml` to configure UI metadata in the Codex app, to set invocation policy, and to declare tool dependencies for a more seamless experience with using a skill.

### Agents 元数据

添加 `agents/openai.yaml` 来配置 Codex 应用中的 UI 元数据、设置调用策略，并声明工具依赖，以获得更流畅的 skill 使用体验。

- UI-facing metadata for skill lists and chips
- Create: human-facing `display_name`, `short_description`, and `default_prompt` by reading the skill
- Generate deterministically by passing the values as `--interface key=value` to `scripts/generate_openai_yaml.py` or `scripts/init_skill.py`
- On updates: validate `agents/openai.yaml` still matches `SKILL.md`; regenerate if stale
- Only include other optional interface fields (icons, brand color) if explicitly provided

- 用于 skill 列表和标签的 UI 元数据
- 创建：通过阅读 skill 生成面向人类的 `display_name`、`short_description` 和 `default_prompt`
- 通过将值作为 `--interface key=value` 传给 `scripts/generate_openai_yaml.py` 或 `scripts/init_skill.py` 来确定性地生成
- 更新时：验证 `agents/openai.yaml` 是否仍与 `SKILL.md` 匹配；如已过时则重新生成
- 仅在明确提供时才包含其他可选界面字段（图标、品牌色）

`allow_implicit_invocation` (default: `true`): When `false`, Codex won't implicitly invoke the skill based on user prompt; explicit `$skill` invocation still works.

`allow_implicit_invocation`（默认值：`true`）：当设为 `false` 时，Codex 不会根据用户 prompt 隐式调用该 skill；显式的 `$skill` 调用仍然有效。

### Core Principles

#### Concise is Key

The context window is a public good. Skills share the context window with everything else Codex needs: system prompt, conversation history, other Skills' metadata, and the actual user request.

**Default assumption: Codex is already very smart.** Only add context Codex doesn't already have. Challenge each piece of information: "Does Codex really need this explanation?" and "Does this paragraph justify its token cost?"

Prefer concise examples over verbose explanations.

### 核心原则

#### 简洁是关键

上下文窗口是公共资源。Skills 与 Codex 需要的其他所有内容共享上下文窗口：系统 prompt、对话历史、其他 Skills 的元数据，以及实际的用户请求。

**默认假设：Codex 已经非常聪明。** 只添加 Codex 尚不具备的上下文。对每条信息都进行质疑："Codex 真的需要这个解释吗？"以及"这段文字值得它消耗的 token 成本吗？"

优先使用简洁的示例而非冗长的解释。

#### Set Appropriate Degrees of Freedom

Match the level of specificity to the task's fragility and variability:

**High freedom (text-based instructions)**: Use when multiple approaches are valid, decisions depend on context, or heuristics guide the approach.

**Medium freedom (pseudocode or scripts with parameters)**: Use when a preferred pattern exists, some variation is acceptable, or configuration affects behavior.

**Low freedom (specific scripts, few parameters)**: Use when operations are fragile and error-prone, consistency is critical, or a specific sequence must be followed.

#### 设定适当的自由度

将指令的具体程度与任务的脆弱性和可变性相匹配：

**高自由度（基于文本的指令）**：当多种方法都有效、决策取决于上下文、或启发式规则指导方法时使用。

**中等自由度（带参数的伪代码或脚本）**：当存在首选模式、可接受一定变化、或配置影响行为时使用。

**低自由度（特定脚本，少量参数）**：当操作脆弱且容易出错、一致性至关重要、或必须遵循特定顺序时使用。

#### Progressive Disclosure Patterns

Keep `SKILL.md` body to the essentials and under 500 lines to minimize context bloat. Split content into separate files when approaching this limit. When splitting out content into other files, it is very important to reference them from `SKILL.md` and describe clearly when to read them.

**Key principle:** When a skill supports multiple variations, frameworks, or options, keep only the core workflow and selection guidance in `SKILL.md`. Move variant-specific details (patterns, examples, configuration) into separate reference files.

#### 渐进式加载模式

将 `SKILL.md` 正文保持在 essentials 以内且不超过 500 行，以减少上下文膨胀。当接近此限制时，将内容拆分到单独的文件中。将内容拆分到其他文件时，务必在 `SKILL.md` 中引用它们，并清晰描述何时读取这些文件。

**关键原则：** 当 skill 支持多种变体、框架或选项时，仅将核心工作流和选择指引保留在 `SKILL.md` 中。将变体特定的细节（模式、示例、配置）移到单独的参考文件中。

**Pattern 1: High-level guide with references**

```markdown
# PDF Processing

## Quick start

Extract text with pdfplumber:
[code example]

## Advanced features

- **Form filling**: See [FORMS.md](FORMS.md) for complete guide
- **API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
- **Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns
```

Codex loads `FORMS.md`, `REFERENCE.md`, or `EXAMPLES.md` only when needed.

**模式 1：带参考文档的高级指南**

```markdown
# PDF Processing

## Quick start

Extract text with pdfplumber:
[code example]

## Advanced features

- **Form filling**: See [FORMS.md](FORMS.md) for complete guide
- **API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
- **Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns
```

Codex 仅在需要时才加载 `FORMS.md`、`REFERENCE.md` 或 `EXAMPLES.md`。

**Pattern 2: Domain-specific organization**

For Skills with multiple domains, organize content by domain to avoid loading irrelevant context:

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

When a user asks about sales metrics, Codex only reads `sales.md`.

**模式 2：按领域组织**

对于包含多个领域的 Skills，按领域组织内容以避免加载不相关的上下文：

```
bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
```

当用户询问销售指标时，Codex 仅读取 `sales.md`。

### Skill Naming

- Use lowercase letters, digits, and hyphens only; normalize user-provided titles to hyphen-case (e.g., "Plan Mode" -> `plan-mode`).
- When generating names, generate a name under 64 characters (letters, digits, hyphens).
- Prefer short, verb-led phrases that describe the action.
- Namespace by tool when it improves clarity or triggering (e.g., `gh-address-comments`, `linear-address-issue`).
- Name the skill folder exactly after the skill name.

### Skill 命名

- 仅使用小写字母、数字和连字符；将用户提供的标题规范化为连字符形式（例如，"Plan Mode" -> `plan-mode`）。
- 生成名称时，生成不超过 64 个字符的名称（字母、数字、连字符）。
- 优先使用描述动作的简短动词短语。
- 当能提高清晰度或触发准确性时，按工具命名空间化（例如，`gh-address-comments`、`linear-address-issue`）。
- skill 文件夹名称与 skill 名称完全一致。

### Forward-testing

To forward-test, launch subagents (子智能体) as a way to stress test the skill with minimal context. Subagents should *not* know that they are being asked to test the skill. They should be treated as an agent asked to perform a task by the user. Prompts to subagents should look like:

```
Use $skill-x at /path/to/skill-x to solve problem y
```

Not:

```
Review the skill at /path/to/skill-x; pretend a user asks you to...
```

### 前向测试

要进行 forward-testing（前向测试），启动 subagents 作为以最小上下文对 skill 进行压力测试的方式。subagents 不应知道它们被要求测试 skill。应将它们视为被用户要求执行任务的 agent。给 subagents 的 prompt 应该类似于：

```
Use $skill-x at /path/to/skill-x to solve problem y
```

而不是：

```
Review the skill at /path/to/skill-x; pretend a user asks you to...
```

Decision rule for forward-testing:

- Err on the side of forward-testing
- Ask for approval if you think there's a risk that forward-testing would:
  - take a long time,
  - require additional approvals from the user, or
  - modify live production systems

前向测试的决策规则：

- 倾向于进行前向测试
- 如果你认为前向测试可能存在以下风险，则请求批准：
  - 耗时较长，
  - 需要用户额外批准，或
  - 修改生产系统

Considerations when forward-testing:

- use fresh threads for independent passes
- pass the skill, and a request in a similar way the user would
- pass raw artifacts, not your conclusions
- avoid showing expected answers or intended fixes
- rebuild context from source artifacts after each iteration
- review the subagent's output and reasoning and emitted artifacts
- avoid leaving artifacts the agent can find on disk between iterations; clean up subagents' artifacts to avoid additional contamination

前向测试时的注意事项：

- 为独立的测试轮次使用全新的会话线程
- 以用户类似的方式传递 skill 和请求
- 传递原始工件，而非你的结论
- 避免展示预期答案或预期修复
- 每次迭代后从源工件重建上下文
- 审查 subagent 的输出、推理和生成的工件
- 避免在迭代之间留下 agent 能在磁盘上发现的工件；清理 subagents 的工件以避免额外污染

If forward-testing only succeeds when subagents see leaked context, tighten the skill or the forward-testing setup before trusting the result.

如果前向测试仅在 subagents 看到泄露的上下文时才成功，则在信任结果之前，先收紧 skill 或前向测试的设置。

### What to Not Include in a Skill

A skill should only contain essential files that directly support its functionality. Do NOT create extraneous documentation or auxiliary files, including:

- `README.md`
- `INSTALLATION_GUIDE.md`
- `QUICK_REFERENCE.md`
- `CHANGELOG.md`
- etc.

### Skill 中不应包含的内容

一个 skill 应仅包含直接支撑其功能的核心文件。不要创建多余的文档或辅助文件，包括：

- `README.md`
- `INSTALLATION_GUIDE.md`
- `QUICK_REFERENCE.md`
- `CHANGELOG.md`
- 等等

The skill should only contain the information needed for an AI agent to do the job at hand. It should not contain auxiliary context about the process that went into creating it, setup and testing procedures, user-facing documentation, etc. Creating additional documentation files just adds clutter and confusion.

skill 应仅包含 AI agent 完成当前工作所需的信息。不应包含关于创建过程的辅助上下文、设置和测试流程、面向用户的文档等。创建额外的文档文件只会增加混乱和困惑。

---

## Codex - Subagents（子代理）

原文链接：https://developers.openai.com/codex/subagents

### Subagents

Codex can run subagent（子代理） workflows（工作流） by spawning specialized agents（代理） in parallel and then collecting their results in one response. This can be particularly helpful for complex tasks that are highly parallel, such as codebase exploration or implementing a multi-step feature plan.

Codex 可以通过并行生成专门的 agent（代理）来运行 subagent（子代理）workflow（工作流），然后在一个响应中收集它们的结果。这对于高度并行的复杂任务特别有帮助，例如代码库探索或实现多步骤功能计划。

With subagent workflows, you can also define your own custom agents with different model configurations and instructions depending on the task.

借助 subagent workflow，你还可以根据任务定义自己的自定义 agent，配置不同的模型和指令。

Current Codex releases enable subagent workflows by default.

当前版本的 Codex 默认启用 subagent workflow。

Subagent activity is currently surfaced in the Codex app and CLI. Visibility in the IDE Extension is coming soon.

Subagent 活动目前可在 Codex 应用和 CLI 中查看。IDE 扩展中的可见性支持即将推出。

Codex only spawns subagents when you explicitly ask it to. Because each subagent does its own model and tool work, subagent workflows consume more tokens than comparable single-agent runs.

Codex 只在你明确要求时才会生成 subagent。由于每个 subagent 都会独立执行模型调用和工具操作，subagent workflow 比同等规模的单 agent 运行消耗更多 token。

In practice, manual triggering means using direct instructions such as "spawn two agents," "delegate this work in parallel," or "use one agent per point."

在实践中，手动触发意味着使用直接指令，例如"生成两个 agent"、"并行委派这些工作"或"每个要点使用一个 agent"。

Codex handles orchestration across agents, including spawning new subagents, routing follow-up instructions, waiting for results, and closing agent threads.

Codex 负责跨 agent 的编排，包括生成新的 subagent、路由后续指令、等待结果以及关闭 agent 线程。

A good subagent prompt（提示词） should explain how to divide the work, whether Codex should wait for all agents before continuing, and what summary or output to return.

一个好的 subagent prompt（提示词）应该说明如何拆分工作、Codex 是否应该等待所有 agent 完成后再继续，以及返回什么样的摘要或输出。

```text
Review this branch with parallel subagents. Spawn one subagent for security risks, one for test gaps, and one for maintainability. Wait for all three, then summarize the findings by category with file references.
```

（示例 prompt：使用并行 subagent 审查此分支。生成一个 subagent 负责安全风险，一个负责测试缺口，一个负责可维护性。等待三者全部完成后，按类别汇总发现并附上文件引用。）

### Choosing models and reasoning

Different agents need different model and reasoning settings.

### 选择模型和推理

不同的 agent 需要不同的模型和推理设置。

If you don't pin a model or `model_reasoning_effort`, Codex can choose a setup that balances intelligence, speed, and price for the task. It may favor `gpt-5.4-mini` for fast scans or a higher-effort `gpt-5.5` configuration for more demanding reasoning. When you want finer control, steer that choice in your prompt or set `model` and `model_reasoning_effort` directly in the agent file.

如果你不指定模型或 `model_reasoning_effort`，Codex 会为任务选择一个在智能、速度和价格之间取得平衡的配置。它可能倾向于在快速扫描时使用 `gpt-5.4-mini`，或在需要更强推理时使用更高 effort 的 `gpt-5.5` 配置。当你需要更精细的控制时，可以在 prompt 中引导这一选择，或者直接在 agent 文件中设置 `model` 和 `model_reasoning_effort`。

For most tasks in Codex, start with `gpt-5.5`. Use `gpt-5.4-mini` when you want a faster, lower-cost option for lighter subagent work. If you have ChatGPT Pro and want near-instant text-only iteration, `gpt-5.3-codex-spark` remains available in research preview.

对于 Codex 中的大多数任务，建议从 `gpt-5.5` 开始。当你需要更快、更低成本的轻量 subagent 工作时，使用 `gpt-5.4-mini`。如果你拥有 ChatGPT Pro 并希望获得近乎即时的纯文本迭代，`gpt-5.3-codex-spark` 在研究预览阶段仍然可用。

- `gpt-5.5`: Start here for demanding agents. It is strongest for ambiguous, multi-step work that needs planning, tool use, validation, and follow-through across a larger context.
- `gpt-5.4`: Use this when a workflow is pinned to GPT-5.4. It combines strong coding, reasoning, tool use, and broader workflows.
- `gpt-5.4-mini`: Use for agents that favor speed and efficiency over depth, such as exploration, read-heavy scans, large-file review, or processing supporting documents. It works well for parallel workers that return distilled results to the main agent.

- `gpt-5.5`：要求较高的 agent 从这里开始。它最适合模糊的、多步骤的工作，需要在更大上下文中进行规划、工具调用、验证和后续跟进。
- `gpt-5.4`：当 workflow 固定使用 GPT-5.4 时使用。它结合了强大的编码、推理、工具调用和更广泛的 workflow 能力。
- `gpt-5.4-mini`：用于优先考虑速度和效率而非深度的 agent，例如探索、大量读取的扫描、大文件审查或处理辅助文档。它非常适合将精炼结果返回给主 agent 的并行 worker。

### Reasoning effort (`model_reasoning_effort`)

- `high`: Use when an agent needs to trace complex logic, check assumptions, or work through edge cases (for example, reviewer or security-focused agents).
- `medium`: A balanced default for most agents.
- `low`: Use when the task is straightforward and speed matters most.

### 推理 effort（`model_reasoning_effort`）

- `high`：当 agent 需要追踪复杂逻辑、检查假设或处理边缘情况时使用（例如 reviewer 或安全相关的 agent）。
- `medium`：大多数 agent 的平衡默认值。
- `low`：当任务简单且速度最重要时使用。

### Managing subagents

- Use `/agent` in the CLI to switch between active agent threads and inspect the ongoing thread.
- Ask Codex directly to steer a running subagent, stop it, or close completed agent threads.

### 管理 subagent

- 在 CLI 中使用 `/agent` 命令切换活跃的 agent 线程并查看正在进行的线程。
- 直接要求 Codex 引导正在运行的 subagent、停止它或关闭已完成的 agent 线程。

### Custom agents

Codex ships with built-in agents:

- `default`: general-purpose fallback agent.
- `worker`: execution-focused agent for implementation and fixes.
- `explorer`: read-heavy codebase exploration agent.

### 自定义 agent

Codex 内置了以下 agent：

- `default`：通用型兜底 agent。
- `worker`：专注于执行的 agent，用于实现和修复。
- `explorer`：以读取为主的代码库探索 agent。

To define your own custom agents, add standalone TOML files under `~/.codex/agents/` for personal agents or `.codex/agents/` for project-scoped agents.

要定义你自己的自定义 agent，可以在 `~/.codex/agents/` 下添加独立的 TOML 文件（个人 agent），或在 `.codex/agents/` 下添加（项目级 agent）。

Each file defines one custom agent. Codex loads these files as configuration layers for spawned sessions, so custom agents can override the same settings as a normal Codex session config. That can feel heavier than a dedicated agent manifest, and the format may evolve as authoring and sharing mature.

每个文件定义一个自定义 agent。Codex 将这些文件作为生成会话的配置层加载，因此自定义 agent 可以覆盖与普通 Codex 会话配置相同的设置。这可能比专用的 agent 清单感觉更重，且格式可能会随着编写和共享的成熟而演进。

### Global settings

Global subagent settings still live under `[agents]` in your configuration.

### 全局设置

全局 subagent 设置仍然位于配置中的 `[agents]` 下。

| Field | Type | Required | Purpose |
| --- | --- | --- | --- |
| `agents.max_threads` | number | No | Concurrent open agent thread cap. |
| `agents.max_depth` | number | No | Spawned agent nesting depth (root session starts at 0). |
| `agents.job_max_runtime_seconds` | number | No | Default timeout per worker for `spawn_agents_on_csv` jobs. |

| 字段 | 类型 | 必填 | 用途 |
| --- | --- | --- | --- |
| `agents.max_threads` | number | No | 并发打开的 agent 线程上限。 |
| `agents.max_depth` | number | No | 生成 agent 的嵌套深度（根会话从 0 开始）。 |
| `agents.job_max_runtime_seconds` | number | No | `spawn_agents_on_csv` 作业中每个 worker 的默认超时时间。 |

- `agents.max_threads` defaults to `6` when you leave it unset.
- `agents.max_depth` defaults to `1`, which allows a direct child agent to spawn but prevents deeper nesting. Keep the default unless you specifically need recursive delegation. Raising this value can turn broad delegation instructions into repeated fan-out, which increases token usage, latency, and local resource consumption. `agents.max_threads` still caps concurrent open threads, but it doesn't remove the cost and predictability risks of deeper recursion.
- `agents.job_max_runtime_seconds` is optional. When you leave it unset, `spawn_agents_on_csv` falls back to its per-call default timeout of 1800 seconds per worker.
- If a custom agent name matches a built-in agent such as `explorer`, your custom agent takes precedence.

- `agents.max_threads` 未设置时默认为 `6`。
- `agents.max_depth` 默认为 `1`，允许生成直接子 agent 但阻止更深层嵌套。除非你确实需要递归委派，否则请保持默认值。提高此值可能将宽泛的委派指令转化为重复的扇出，从而增加 token 用量、延迟和本地资源消耗。`agents.max_threads` 仍然限制并发打开的线程数，但它无法消除更深递归带来的成本和可预测性风险。
- `agents.job_max_runtime_seconds` 是可选的。未设置时，`spawn_agents_on_csv` 回退到每次调用默认的 1800 秒/worker 超时。
- 如果自定义 agent 的名称与内置 agent（如 `explorer`）匹配，你的自定义 agent 优先。

### Custom agent file schema

| Field | Type | Required | Purpose |
| --- | --- | --- | --- |
| `name` | string | Yes | Agent name Codex uses when spawning or referring to this agent. |
| `description` | string | Yes | Human-facing guidance for when Codex should use this agent. |
| `developer_instructions` | string | Yes | Core instructions that define the agent's behavior. |
| `nickname_candidates` | string[] | No | Optional pool of display nicknames for spawned agents. |

### 自定义 agent 文件 schema

| 字段 | 类型 | 必填 | 用途 |
| --- | --- | --- | --- |
| `name` | string | Yes | Codex 在生成或引用此 agent 时使用的名称。 |
| `description` | string | Yes | 面向人类的指导，说明 Codex 何时应使用此 agent。 |
| `developer_instructions` | string | Yes | 定义 agent 行为的核心指令。 |
| `nickname_candidates` | string[] | No | 生成 agent 的可选显示昵称池。 |

You can also include other supported `config.toml` keys in a custom agent file, such as `model`, `model_reasoning_effort`, `sandbox_mode`, `mcp_servers`, and `skills.config`.

你还可以在自定义 agent 文件中包含其他受支持的 `config.toml` 键，例如 `model`、`model_reasoning_effort`、`sandbox_mode`、`mcp_servers` 和 `skills.config`。

Codex identifies the custom agent by its `name` field. Matching the filename to the agent name is the simplest convention, but the `name` field is the source of truth.

Codex 通过 `name` 字段标识自定义 agent。将文件名与 agent 名称匹配是最简单的约定，但 `name` 字段是真正的标识依据。

### Example custom agents

- `pr_explorer` maps the codebase and gathers evidence.
- `reviewer` looks for correctness, security, and test risks.
- `docs_researcher` checks framework or API documentation through a dedicated MCP server.

### 自定义 agent 示例

- `pr_explorer`：映射代码库并收集证据。
- `reviewer`：查找正确性、安全性和测试风险。
- `docs_researcher`：通过专用 MCP server 检查框架或 API 文档。

`.codex/agents/pr-explorer.toml`:

```
name = "pr_explorer"
description = "Read-only codebase explorer for gathering evidence before changes are proposed."
model = "gpt-5.3-codex-spark"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
developer_instructions = """
Stay in exploration mode.
Trace the real execution path, cite files and symbols, and avoid proposing fixes unless the parent agent asks for them.
Prefer fast search and targeted file reads over broad scans.
"""
```

（此示例定义了一个只读代码库探索 agent，使用 `gpt-5.3-codex-spark` 模型和 medium 推理 effort，在探索模式下追踪真实执行路径、引用文件和符号，除非父 agent 要求否则不提出修复建议，优先使用快速搜索和定向文件读取而非大范围扫描。）

```
name = "code_mapper"
description = "Read-only codebase explorer for locating relevant code and touchpoints."
model = "gpt-5.4-mini"
model_reasoning_effort = "medium"
sandbox_mode = "read-only"
```

（此示例定义了一个只读代码库探索 agent，用于定位相关代码和触点，使用 `gpt-5.4-mini` 模型和 medium 推理 effort。）

### Process CSV batches with subagents (experimental)

This workflow is experimental and may change as subagent support evolves. Use `spawn_agents_on_csv` when you have many similar tasks that map to one row per work item. Codex reads the CSV, spawns one worker subagent per row, waits for the full batch to finish, and exports the combined results to CSV.

### 使用 subagent 批量处理 CSV（实验性）

此 workflow 是实验性的，可能随 subagent 支持的演进而变化。当你有许多相似任务且每个任务映射到一行工作项时，使用 `spawn_agents_on_csv`。Codex 读取 CSV，为每行生成一个 worker subagent，等待整批完成，并将合并结果导出为 CSV。

The tool accepts:

- `csv_path` for the source CSV
- `instruction` for the worker prompt template, using `{column_name}` placeholders
- `id_column` when you want stable item ids from a specific column
- `output_schema` when each worker should return a JSON object with a fixed shape
- `output_csv_path`, `max_concurrency`, and `max_runtime_seconds` for job control

该工具接受以下参数：

- `csv_path`：源 CSV 文件路径
- `instruction`：worker prompt 模板，使用 `{column_name}` 占位符
- `id_column`：当你希望从特定列获取稳定的项目 id 时使用
- `output_schema`：当每个 worker 应返回固定结构的 JSON 对象时使用
- `output_csv_path`、`max_concurrency` 和 `max_runtime_seconds`：用于作业控制

Example prompt:

```
Create /tmp/components.csv with columns path,owner and one row per frontend component.

Then call spawn_agents_on_csv with:
- csv_path: /tmp/components.csv
- id_column: path
- instruction: "Review {path} owned by {owner}. Return JSON with keys path, risk, summary, and follow_up via report_agent_job_result."
- output_csv_path: /tmp/components-review.csv
- output_schema: an object with required string fields path, risk, summary, and follow_up
```

（此示例 prompt 先创建一个包含 path 和 owner 列的 CSV 文件，每行对应一个前端组件，然后调用 `spawn_agents_on_csv`，为每行生成一个 worker subagent 来审查对应组件，并通过 `report_agent_job_result` 返回包含 path、risk、summary 和 follow_up 字段的 JSON 结果，最终导出到 `/tmp/components-review.csv`。）

---

## Codex - Code review in GitHub（GitHub 代码审查）

原文链接：https://developers.openai.com/codex/integrations/github

### Code review in GitHub

Use Codex code review (代码审查) to get another high-signal review pass on GitHub pull requests (拉取请求). Codex reviews the pull request diff, follows your repository guidance, and posts a standard GitHub code review focused on serious issues.

使用 Codex code review（代码审查）为 GitHub pull request（拉取请求）增加一道高信号审查。Codex 会审查 pull request 的 diff，遵循仓库内的指导规范，并提交一份聚焦于严重问题的标准 GitHub code review。

### Before you begin

- Codex cloud set up for the repository you want to review.
- Access to Codex code review settings.
- An `AGENTS.md` file if you want Codex to follow repository-specific review guidance.

- 已为你想审查的仓库配置好 Codex cloud（Codex 云端环境）。
- 拥有 Codex code review 设置的访问权限。
- 如果你希望 Codex 遵循仓库专属的审查指导，需准备一个 `AGENTS.md` 文件。

### Set up Codex code review

1. Set up Codex cloud.
2. Go to Codex settings.
3. Turn on Code review for your repository.

1. 配置好 Codex cloud。
2. 进入 Codex 设置。
3. 为你的仓库开启 Code review。

### Request a Codex review

1. In a pull request comment, mention `@codex review`.
2. Wait for Codex to react (👀) and post a review.

1. 在 pull request 评论中提及 `@codex review`。
2. 等待 Codex 做出反应（👀）并提交审查。

Codex posts a review on the pull request, just like a teammate would. In GitHub, Codex flags only P0 and P1 issues so review comments stay focused on high-priority risks.

Codex 会像队友一样在 pull request 上提交审查。在 GitHub 中，Codex 只标记 P0 和 P1 级别的问题，以确保审查评论聚焦于高优先级风险。

### Enable automatic reviews

If you want Codex to review every pull request automatically, turn on Automatic reviews in Codex settings.

如果你希望 Codex 自动审查每一个 pull request，请在 Codex 设置中开启 Automatic reviews。

Codex will post a review whenever someone opens a new PR for review, without needing an `@codex review` comment.

当有人提交新的 PR 请求审查时，Codex 会自动提交一份审查，无需 `@codex review` 评论。

### Customize what Codex reviews

Codex searches your repository for `AGENTS.md` files and follows any Review guidelines you include.

Codex 会在你的仓库中搜索 `AGENTS.md` 文件，并遵循其中包含的 Review guidelines。

To set guidelines for a repository, add or update a top-level `AGENTS.md` with a section like this:

要为仓库设置指导规范，请添加或更新顶层的 `AGENTS.md`，加入如下小节：

```md
## Review guidelines

- Don't log PII.
- Verify that authentication middleware wraps every route.
```

Codex applies guidance from the closest `AGENTS.md` to each changed file. You can place more specific instructions deeper in the tree when particular packages need extra scrutiny.

Codex 会将距离每个变更文件最近的 `AGENTS.md` 中的指导应用于该文件。当某些特定 package 需要额外审查时，你可以在目录树更深处放置更具体的指令。

For a one-off focus, add it to your pull request comment:

`@codex review for security regressions`

如需一次性聚焦某个方面，可将其添加到你的 pull request 评论中：

`@codex review for security regressions`

If you want Codex to flag typos in documentation, add guidance in `AGENTS.md` (for example, "Treat typos in docs as P1.").

如果你希望 Codex 标记文档中的拼写错误，请在 `AGENTS.md` 中添加指导（例如："Treat typos in docs as P1."）。

### Ask Codex to fix issues

After Codex posts a review, you can ask it to fix issues in the same pull request by leaving another comment:

在 Codex 提交审查后，你可以通过留下另一条评论，要求它在同一个 pull request 中修复问题：

`@codex fix`

Codex starts a cloud task (云端任务) with the pull request as context and can push a fix back to the branch when it has permission to do so.

Codex 会以该 pull request 为上下文启动一个 cloud task（云端任务），并在拥有权限时将修复推送到对应分支。

If you mention `@codex` in a comment with anything other than `review`, Codex starts a cloud task using your pull request as context.

如果你在评论中提及 `@codex` 但内容不是 `review`，Codex 会以你的 pull request 为上下文启动一个 cloud task。

### Troubleshoot code review

If Codex doesn't react or post a review:

如果 Codex 没有反应或未提交审查：

- Confirm you turned on Code review for the repository in Codex settings.
- Confirm the pull request belongs to a repository with Codex cloud set up.
- Use the exact trigger `@codex review` in a pull request comment.
- For automatic reviews, check that you turned on Automatic reviews and that the pull request event matches your review trigger settings.

- 确认你已在 Codex 设置中为该仓库开启了 Code review。
- 确认该 pull request 属于已配置 Codex cloud 的仓库。
- 在 pull request 评论中使用准确的触发词 `@codex review`。
- 对于自动审查，请检查你是否开启了 Automatic reviews，并且 pull request 事件与你的审查触发设置相匹配。

---

## Codex - CLI features（CLI 功能）— 访问失败

原文链接：https://developers.openai.com/codex/cli/features

> **访问失败 (ACCESS FAILED)**

原文链接：https://developers.openai.com/codex/cli/features

访问状态：HTTP 403 Forbidden — 该页面无法通过任何方式（直接抓取、Jina Reader、搜索摘要、GitHub 原始 markdown、镜像站）获取到内容。

由于无法访问原始资料，本节内容缺失，未做任何翻译。如需该文档的翻译，请手动提供页面内容或使用可访问的网络环境重新抓取。


---

## Claude Code - Automate actions with hooks（用 hook 自动化操作）

原文链接：https://code.claude.com/docs/en/hooks-guide

TITLE: Claude Code - Automate actions with hooks
URL: https://code.claude.com/docs/en/hooks-guide
ACCESS: SUCCESS

---TRANSLATION START---

### Automate actions with hooks

> Run shell commands automatically when Claude Code edits files, finishes tasks, or needs input. Format code, send notifications, validate commands, and enforce project rules.

> 当 Claude Code 编辑文件、完成任务或需要输入时，自动运行 shell 命令。可以格式化代码、发送通知、校验命令，并强制执行项目规则。

Hooks are user-defined shell commands that execute at specific points in Claude Code's lifecycle. They provide deterministic control over Claude Code's behavior, ensuring certain actions always happen rather than relying on the LLM to choose to run them. Use hooks to enforce project rules, automate repetitive tasks, and integrate Claude Code with your existing tools.

hook（钩子，在特定生命周期节点自动执行的用户自定义 shell 命令）在 Claude Code 生命周期的特定节点执行。它们提供对 Claude Code 行为的确定性控制，确保某些操作必定发生，而非依赖 LLM 自行决定是否执行。可以使用 hook 来强制执行项目规则、自动化重复任务，并将 Claude Code 与你现有的工具集成。

For decisions that require judgment rather than deterministic rules, you can also use prompt-based hooks or agent-based hooks that use a Claude model to evaluate conditions.

对于需要判断力而非确定性规则的决策，你也可以使用基于 prompt（提示词）的 hook 或基于 agent（智能体）的 hook，它们会调用 Claude 模型来评估条件。

For other ways to extend Claude Code, see skills for giving Claude additional instructions and executable commands, subagents for running tasks in isolated contexts, and plugins for packaging extensions to share across projects.

扩展 Claude Code 的其他方式包括：skill（技能，为 Claude 提供额外指令和可执行命令）、subagent（子智能体，在隔离上下文中运行任务）、以及 plugin（插件，打包扩展以便跨项目共享）。

<Tip>
This guide covers common use cases and how to get started. For full event schemas, JSON input/output formats, and advanced features like async hooks and MCP tool hooks, see the Hooks reference.
</Tip>

<提示>
本指南涵盖常见用例和入门方法。如需完整的事件 schema、JSON 输入/输出格式以及异步 hook 和 MCP 工具 hook 等高级功能，请参阅 Hooks 参考文档。
</提示>

### Set up your first hook

To create a hook, add a `hooks` block to a settings file. This walkthrough creates a desktop notification hook, so you get alerted whenever Claude is waiting for your input instead of watching the terminal.

要创建 hook，需在 settings 文件中添加一个 `hooks` 块。本示例将创建一个桌面通知 hook，这样每当 Claude 等待你的输入时你都会收到提醒，而不必一直盯着终端。

**Step 1: Add the hook to your settings**

Open `~/.claude/settings.json` and add a `Notification` hook. The example below uses `osascript` for macOS; see Get notified when Claude needs input for Linux and Windows commands.

打开 `~/.claude/settings.json` 并添加一个 `Notification` hook。以下示例在 macOS 上使用 `osascript`；Linux 和 Windows 的命令请参阅"在 Claude 需要输入时获取通知"一节。

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code needs your attention\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

If your settings file already has a `hooks` key, add `Notification` as a sibling of the existing event keys rather than replacing the whole object. Each event name is a key inside the single `hooks` object:

如果你的 settings 文件已有 `hooks` 键，应将 `Notification` 添加为现有事件键的同级项，而非替换整个对象。每个事件名都是 `hooks` 对象内的一个键：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [{ "type": "command", "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write" }]
      }
    ],
    "Notification": [
      {
        "matcher": "",
        "hooks": [{ "type": "command", "command": "osascript -e 'display notification \"Claude Code needs your attention\" with title \"Claude Code\"'" }]
      }
    ]
  }
}
```

You can also ask Claude to write the hook for you by describing what you want in the CLI.

你也可以在 CLI 中描述你的需求，让 Claude 帮你编写 hook。

**Step 2: Verify the configuration**

Type `/hooks` to open the hooks browser. You'll see a list of all available hook events, with a count next to each event that has hooks configured. Select `Notification` to confirm your new hook appears in the list. Selecting the hook shows its details: the event, matcher, type, source file, and command.

输入 `/hooks` 打开 hook 浏览器。你将看到所有可用 hook 事件的列表，已配置 hook 的事件旁边会显示数量。选择 `Notification` 确认你的新 hook 出现在列表中。选中 hook 后会显示其详情：事件、matcher、类型、来源文件和命令。

**Step 3: Test the hook**

Press `Esc` to return to the CLI. Ask Claude to do something that requires permission, then switch away from the terminal. You should receive a desktop notification.

按 `Esc` 返回 CLI。让 Claude 执行一个需要权限的操作，然后切换离开终端。你应该会收到一个桌面通知。

<Tip>
The `/hooks` menu is read-only. To add, modify, or remove hooks, edit your settings JSON directly or ask Claude to make the change.
</Tip>

<提示>
`/hooks` 菜单是只读的。要添加、修改或删除 hook，请直接编辑 settings JSON 文件，或让 Claude 来修改。
</提示>

### What you can automate

Hooks let you run code at key points in Claude Code's lifecycle: format files after edits, block commands before they execute, send notifications when Claude needs input, inject context at session start, and more. For the full list of hook events, see the Hooks reference.

hook 允许你在 Claude Code 生命周期的关键节点运行代码：编辑后格式化文件、执行前阻止命令、Claude 需要输入时发送通知、会话开始时注入上下文等。完整的 hook 事件列表请参阅 Hooks 参考文档。

Each example includes a ready-to-use configuration block that you add to a settings file.

每个示例都包含一个可直接使用的配置块，你只需将其添加到 settings 文件中。

For a production example of hooks that run a separate model review and feed findings back into the session, see how the `security-guidance` plugin integrates with Claude Code.

如需查看运行独立模型审查并将结果反馈到会话中的 hook 生产示例，请参阅 `security-guidance` 插件如何与 Claude Code 集成。

### Get notified when Claude needs input

Get a desktop notification whenever Claude finishes working and needs your input, so you can switch to other tasks without checking the terminal.

每当 Claude 完成工作并需要你的输入时，获取一个桌面通知，这样你可以切换到其他任务而无需频繁查看终端。

This hook uses the `Notification` event, which fires when Claude is waiting for input or permission. Each tab below uses the platform's native notification command. Add this to `~/.claude/settings.json`:

此 hook 使用 `Notification` 事件，该事件在 Claude 等待输入或权限时触发。以下各标签页使用各平台的原生通知命令。将配置添加到 `~/.claude/settings.json`：

**macOS:**

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'display notification \"Claude Code needs your attention\" with title \"Claude Code\"'"
          }
        ]
      }
    ]
  }
}
```

If no notification appears: `osascript` routes notifications through the built-in Script Editor app. If Script Editor doesn't have notification permission, the command fails silently, and macOS won't prompt you to grant it. Run this in Terminal once to make Script Editor appear in your notification settings:

如果通知未出现：`osascript` 通过内置的 Script Editor 应用发送通知。如果 Script Editor 没有通知权限，命令会静默失败，macOS 也不会提示你授权。在终端中运行一次以下命令，使 Script Editor 出现在通知设置中：

```bash
osascript -e 'display notification "test"'
```

Nothing will appear yet. Open System Settings > Notifications, find Script Editor in the list, and turn on Allow Notifications. Run the command again to confirm the test notification appears.

此时还不会出现任何通知。打开系统设置 > 通知，在列表中找到 Script Editor，开启"允许通知"。再次运行该命令，确认测试通知出现。

**Linux:**

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "notify-send 'Claude Code' 'Claude Code needs your attention'"
          }
        ]
      }
    ]
  }
}
```

**Windows (PowerShell):**

```json
{
  "hooks": {
    "Notification": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "powershell.exe -Command \"[System.Reflection.Assembly]::LoadWithPartialName('System.Windows.Forms'); [System.Windows.Forms.MessageBox]::Show('Claude Code needs your attention', 'Claude Code')\""
          }
        ]
      }
    ]
  }
}
```

The empty `matcher` fires on all notification types. To fire only on specific events, set it to one of these values:

空的 `matcher` 会对所有通知类型触发。若只需对特定事件触发，可设置为以下值之一：

| Matcher                | Fires when                                                                                               |
| :--------------------- | :------------------------------------------------------------------------------------------------------- |
| `permission_prompt`    | Claude needs you to approve a tool use                                                                   |
| `idle_prompt`          | Claude is done and waiting for your next prompt                                                          |
| `auth_success`         | Authentication completes                                                                                 |
| `elicitation_dialog`   | An MCP server opens an elicitation form                                                                  |
| `elicitation_complete` | An MCP elicitation form is submitted or dismissed                                                        |
| `elicitation_response` | An MCP elicitation response is sent back to the server                                                   |
| `agent_needs_input`    | A background session starts waiting on your input. Fires only while agent view is open                   |
| `agent_completed`      | A background session finishes or fails. Fires only while agent view is open                              |

| Matcher                | 触发时机                                                                                                  |
| :--------------------- | :------------------------------------------------------------------------------------------------------- |
| `permission_prompt`    | Claude 需要你批准某个工具调用                                                                             |
| `idle_prompt`          | Claude 完成工作，等待你的下一条 prompt                                                                    |
| `auth_success`         | 认证完成                                                                                                  |
| `elicitation_dialog`   | MCP 服务器打开了 elicitation（信息采集）表单                                                              |
| `elicitation_complete` | MCP elicitation 表单被提交或关闭                                                                          |
| `elicitation_response` | MCP elicitation 响应已发送回服务器                                                                        |
| `agent_needs_input`    | 后台会话开始等待你的输入。仅在 agent view（智能体视图）打开时触发                                          |
| `agent_completed`      | 后台会话完成或失败。仅在 agent view 打开时触发                                                            |

The `agent_needs_input` and `agent_completed` matchers require Claude Code v2.1.198 or later.

`agent_needs_input` 和 `agent_completed` matcher 需要 Claude Code v2.1.198 或更高版本。

Type `/hooks` and select `Notification` to confirm the hook is registered. For the full event schema, see the Notification reference.

输入 `/hooks` 并选择 `Notification` 以确认 hook 已注册。完整的事件 schema 请参阅 Notification 参考文档。

### Auto-format code after edits

Automatically run Prettier on every file Claude edits, so formatting stays consistent without manual intervention.

在 Claude 编辑的每个文件上自动运行 Prettier，使格式化保持一致而无需手动干预。

This hook uses the `PostToolUse` event with an `Edit|Write` matcher, so it runs only after file-editing tools. The command extracts the edited file path with `jq` and passes it to Prettier. Add this to `.claude/settings.json` in your project root:

此 hook 使用 `PostToolUse` 事件配合 `Edit|Write` matcher，因此仅在文件编辑工具运行后触发。命令通过 `jq` 提取被编辑的文件路径并传递给 Prettier。将以下内容添加到项目根目录的 `.claude/settings.json`：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r '.tool_input.file_path' | xargs npx prettier --write"
          }
        ]
      }
    ]
  }
}
```

On Claude Code v2.1.191 or later you can also write the matcher as `Edit,Write`, since `|` and `,` are interchangeable list separators for tool-name matchers on those versions.

在 Claude Code v2.1.191 或更高版本中，你也可以将 matcher 写为 `Edit,Write`，因为在这些版本中 `|` 和 `,` 对于工具名 matcher 是可互换的列表分隔符。

<Note>
The Bash examples on this page use `jq` for JSON parsing. Install it with `brew install jq` on macOS, `apt-get install jq` on Debian and Ubuntu, or see jq downloads.
</Note>

<注意>
本页的 Bash 示例使用 `jq` 进行 JSON 解析。在 macOS 上用 `brew install jq` 安装，在 Debian 和 Ubuntu 上用 `apt-get install jq` 安装，或参阅 jq 下载页面。
</注意>

### Block edits to protected files

Prevent Claude from modifying sensitive files like `.env`, `package-lock.json`, or anything in `.git/`. Claude receives feedback explaining why the edit was blocked, so it can adjust its approach.

防止 Claude 修改 `.env`、`package-lock.json` 或 `.git/` 中的任何内容等敏感文件。Claude 会收到解释编辑被阻止原因的反馈，从而调整其方法。

This example uses a separate script file that the hook calls. The script checks the target file path against a list of protected patterns and exits with code 2 to block the edit.

此示例使用一个单独的脚本文件供 hook 调用。脚本将目标文件路径与受保护模式列表进行比对，若匹配则以退出码 2 退出以阻止编辑。

**Step 1: Create the hook script**

Save this to `.claude/hooks/protect-files.sh`:

将以下内容保存到 `.claude/hooks/protect-files.sh`：

```bash
#!/bin/bash
# protect-files.sh

INPUT=$(cat)
FILE_PATH=$(echo "$INPUT" | jq -r '.tool_input.file_path // empty')

PROTECTED_PATTERNS=(".env" "package-lock.json" ".git/")

for pattern in "${PROTECTED_PATTERNS[@]}"; do
  if [[ "$FILE_PATH" == *"$pattern"* ]]; then
    echo "Blocked: $FILE_PATH matches protected pattern '$pattern'" >&2
    exit 2
  fi
done

exit 0
```

**Step 2: Make the script executable on macOS and Linux**

Hook scripts must be executable for Claude Code to run them:

Hook 脚本必须具有可执行权限，Claude Code 才能运行它们：

```bash
chmod +x .claude/hooks/protect-files.sh
```

**Step 3: Register the hook**

Add a `PreToolUse` hook to `.claude/settings.json` that runs the script before any `Edit` or `Write` tool call:

在 `.claude/settings.json` 中添加一个 `PreToolUse` hook，在任何 `Edit` 或 `Write` 工具调用之前运行该脚本：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/protect-files.sh"
          }
        ]
      }
    ]
  }
}
```

### Re-inject context after compaction

When Claude's context window fills up, compaction summarizes the conversation to free space. This can lose important details. Use a `SessionStart` hook with a `compact` matcher to re-inject critical context after every compaction.

当 Claude 的上下文窗口填满时，compaction（压缩）会总结对话以释放空间。这可能会丢失重要细节。使用带 `compact` matcher 的 `SessionStart` hook，在每次压缩后重新注入关键上下文。

Any text your command writes to stdout is added to Claude's context. This example reminds Claude of project conventions and recent work. Add this to `.claude/settings.json` in your project root:

你的命令写入 stdout 的任何文本都会被添加到 Claude 的上下文中。以下示例提醒 Claude 项目约定和近期工作。将以下内容添加到项目根目录的 `.claude/settings.json`：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": "compact",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Reminder: use Bun, not npm. Run bun test before committing. Current sprint: auth refactor.'"
          }
        ]
      }
    ]
  }
}
```

You can replace the `echo` with any command that produces dynamic output, like `git log --oneline -5` to show recent commits. For injecting context on every session start, consider using CLAUDE.md instead. For environment variables, see `CLAUDE_ENV_FILE` in the reference.

你可以将 `echo` 替换为任何产生动态输出的命令，例如 `git log --oneline -5` 来显示最近的提交。如果需要在每次会话启动时注入上下文，考虑改用 CLAUDE.md。关于环境变量，请参阅参考文档中的 `CLAUDE_ENV_FILE`。

### Audit configuration changes

Track when settings or skills files change during a session. The `ConfigChange` event fires when an external process or editor modifies a configuration file, so you can log changes for compliance or block unauthorized modifications.

跟踪会话期间 settings 或 skills 文件的变更。`ConfigChange` 事件在外部进程或编辑器修改配置文件时触发，因此你可以记录变更以供合规审计，或阻止未经授权的修改。

This example appends each change to an audit log. Add this to `~/.claude/settings.json`:

此示例将每次变更追加到审计日志中。将以下内容添加到 `~/.claude/settings.json`：

```json
{
  "hooks": {
    "ConfigChange": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "jq -c '{timestamp: now | todate, source: .source, file: .file_path}' >> ~/claude-config-audit.log"
          }
        ]
      }
    ]
  }
}
```

The matcher filters by configuration type: `user_settings`, `project_settings`, `local_settings`, `policy_settings`, or `skills`. To block a change from taking effect, exit with code 2 or return `{"decision": "block"}`. See the ConfigChange reference for the full input schema.

matcher 按配置类型过滤：`user_settings`、`project_settings`、`local_settings`、`policy_settings` 或 `skills`。要阻止变更生效，以退出码 2 退出或返回 `{"decision": "block"}`。完整的输入 schema 请参阅 ConfigChange 参考文档。

### Reload environment when directory or files change

Some projects set different environment variables depending on which directory you are in. Tools like direnv do this automatically in your shell, but Claude's Bash tool doesn't pick up those changes on its own.

某些项目会根据所在目录设置不同的环境变量。direnv 等工具会在你的 shell 中自动完成此操作，但 Claude 的 Bash 工具不会自动获取这些变更。

Pairing a `SessionStart` hook with a `CwdChanged` hook fixes this. `SessionStart` loads the variables for the directory you launch in, and `CwdChanged` reloads them each time Claude changes directory. Both write to `CLAUDE_ENV_FILE`, which Claude Code runs as a script preamble before each Bash command. Add this to `~/.claude/settings.json`:

将 `SessionStart` hook 与 `CwdChanged` hook 配合使用可以解决此问题。`SessionStart` 加载你启动目录中的变量，`CwdChanged` 在 Claude 每次切换目录时重新加载它们。两者都写入 `CLAUDE_ENV_FILE`，Claude Code 会在每条 Bash 命令前将其作为脚本前导执行。将以下内容添加到 `~/.claude/settings.json`：

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "direnv export bash > \"$CLAUDE_ENV_FILE\""
          }
        ]
      }
    ],
    "CwdChanged": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "direnv export bash > \"$CLAUDE_ENV_FILE\""
          }
        ]
      }
    ]
  }
}
```

Run `direnv allow` once in each directory that has an `.envrc` so direnv is permitted to load it. If you use devbox or nix instead of direnv, the same pattern works with `devbox shellenv` or `devbox global shellenv` in place of `direnv export bash`.

在每个含有 `.envrc` 的目录中运行一次 `direnv allow`，使 direnv 被允许加载它。如果你使用 devbox 或 nix 而非 direnv，同样的模式也适用，只需将 `direnv export bash` 替换为 `devbox shellenv` 或 `devbox global shellenv`。

To react to specific files instead of every directory change, use `FileChanged` with a `matcher` listing the filenames to watch, separated by `|`. When building the watch list, Claude Code splits this value into literal filenames rather than evaluating it as a regex. See FileChanged for how the same value also filters which hook groups run when a file changes. This example watches `.envrc` and `.env` in the working directory:

若要对特定文件而非每次目录变更做出反应，可使用 `FileChanged` 配合 `matcher` 列出要监视的文件名，以 `|` 分隔。在构建监视列表时，Claude Code 会将此值拆分为字面文件名，而非将其作为正则表达式求值。关于该值如何同时过滤文件变更时运行的 hook 组，请参阅 FileChanged。以下示例监视工作目录中的 `.envrc` 和 `.env`：

```json
{
  "hooks": {
    "FileChanged": [
      {
        "matcher": ".envrc|.env",
        "hooks": [
          {
            "type": "command",
            "command": "direnv export bash > \"$CLAUDE_ENV_FILE\""
          }
        ]
      }
    ]
  }
}
```

See the CwdChanged and FileChanged reference entries for input schemas, `watchPaths` output, and `CLAUDE_ENV_FILE` details.

关于输入 schema、`watchPaths` 输出和 `CLAUDE_ENV_FILE` 的详细信息，请参阅 CwdChanged 和 FileChanged 参考条目。

### Auto-approve specific permission prompts

Skip the approval dialog for tool calls you always allow. This example auto-approves `ExitPlanMode`, the tool Claude calls when it finishes presenting a plan and asks to proceed, so you aren't prompted every time a plan is ready.

对你始终允许的工具调用跳过审批对话框。此示例自动批准 `ExitPlanMode`——即 Claude 在展示计划完毕并请求继续时调用的工具——这样你不必在每次计划就绪时都被提示。

Unlike the exit-code examples above, auto-approval requires your hook to write a JSON decision to stdout. A `PermissionRequest` hook fires when Claude Code is about to show a permission dialog, and returning `"behavior": "allow"` answers it on your behalf.

与上述退出码示例不同，自动批准需要你的 hook 向 stdout 写入一个 JSON 决策。`PermissionRequest` hook 在 Claude Code 即将显示权限对话框时触发，返回 `"behavior": "allow"` 即代表你批准了该请求。

The matcher scopes the hook to `ExitPlanMode` only, so no other prompts are affected. Add this to `~/.claude/settings.json`:

matcher 将 hook 限定于 `ExitPlanMode`，因此不影响其他提示。将以下内容添加到 `~/.claude/settings.json`：

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "matcher": "ExitPlanMode",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"hookSpecificOutput\": {\"hookEventName\": \"PermissionRequest\", \"decision\": {\"behavior\": \"allow\"}}}'"
          }
        ]
      }
    ]
  }
}
```

When the hook approves, Claude Code exits plan mode and restores whatever permission mode was active before you entered plan mode. The transcript shows "Allowed by PermissionRequest hook" where the dialog would have appeared. The hook path always keeps the current conversation: it can't clear context and start a fresh implementation session the way the dialog can.

当 hook 批准时，Claude Code 退出计划模式并恢复进入计划模式之前的活动权限模式。记录中会在对话框原本出现的位置显示 "Allowed by PermissionRequest hook"。hook 路径始终保留当前对话：它无法像对话框那样清除上下文并开始全新的实施会话。

To set a specific permission mode instead, your hook's output can include an `updatedPermissions` array with a `setMode` entry. The `mode` value is any permission mode like `default`, `acceptEdits`, or `bypassPermissions`, and `destination: "session"` applies it for the current session only.

若要改为设置特定的权限模式，你的 hook 输出可以包含一个带 `setMode` 条目的 `updatedPermissions` 数组。`mode` 值可以是任何权限模式，如 `default`、`acceptEdits` 或 `bypassPermissions`，`destination: "session"` 表示仅对当前会话生效。

<Note>
`bypassPermissions` only applies if the session was launched with bypass mode already available: `--dangerously-skip-permissions`, `--permission-mode bypassPermissions`, `--allow-dangerously-skip-permissions`, or `permissions.defaultMode: "bypassPermissions"` in settings, and not disabled by `permissions.disableBypassPermissionsMode`. It is never persisted as `defaultMode`.
</Note>

<注意>
`bypassPermissions` 仅在会话启动时已具备绕过模式的情况下才生效：`--dangerously-skip-permissions`、`--permission-mode bypassPermissions`、`--allow-dangerously-skip-permissions`，或 settings 中的 `permissions.defaultMode: "bypassPermissions"`，且未被 `permissions.disableBypassPermissionsMode` 禁用。它永远不会被持久化为 `defaultMode`。
</注意>

To switch the session to `acceptEdits`, your hook writes this JSON to stdout:

要将会话切换为 `acceptEdits`，你的 hook 需向 stdout 写入以下 JSON：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PermissionRequest",
    "decision": {
      "behavior": "allow",
      "updatedPermissions": [
        { "type": "setMode", "mode": "acceptEdits", "destination": "session" }
      ]
    }
  }
}
```

Keep the matcher as narrow as possible. Matching on `.*` or leaving the matcher empty would auto-approve every permission prompt, including file writes and shell commands. See the PermissionRequest reference for the full set of decision fields.

请将 matcher 尽可能缩小范围。使用 `.*` 匹配或留空 matcher 会自动批准每个权限提示，包括文件写入和 shell 命令。完整的决策字段请参阅 PermissionRequest 参考文档。

### How hooks work

Hook events fire at specific lifecycle points in Claude Code. When an event fires, all matching hooks run in parallel, and identical hook commands are automatically deduplicated. The table below shows each event and when it triggers:

hook 事件在 Claude Code 的特定生命周期节点触发。当事件触发时，所有匹配的 hook 并行运行，相同的 hook 命令会自动去重。下表展示了每个事件及其触发时机：

| Event                 | When it fires                                                                                                                                          |
| :-------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SessionStart`        | When a session begins or resumes                                                                                                                       |
| `Setup`               | When you start Claude Code with `--init-only`, or with `--init` or `--maintenance` in `-p` mode. For one-time preparation in CI or scripts             |
| `UserPromptSubmit`    | When you submit a prompt, before Claude processes it                                                                                                   |
| `UserPromptExpansion` | When a user-typed command expands into a prompt, before it reaches Claude. Can block the expansion                                                     |
| `PreToolUse`          | Before a tool call executes. Can block it                                                                                                              |
| `PermissionRequest`   | When a permission dialog appears                                                                                                                       |
| `PermissionDenied`    | When a tool call is denied by the auto mode classifier. Return `{retry: true}` to tell the model it may retry the denied tool call                     |
| `PostToolUse`         | After a tool call succeeds                                                                                                                             |
| `PostToolUseFailure`  | After a tool call fails                                                                                                                                |
| `PostToolBatch`       | After a full batch of parallel tool calls resolves, before the next model call                                                                         |
| `Notification`        | When Claude Code sends a notification                                                                                                                  |
| `MessageDisplay`      | While assistant message text is displayed                                                                                                              |
| `SubagentStart`       | When a subagent is spawned                                                                                                                             |
| `SubagentStop`        | When a subagent finishes                                                                                                                               |
| `TaskCreated`         | When a task is being created via `TaskCreate`                                                                                                          |
| `TaskCompleted`       | When a task is being marked as completed                                                                                                               |
| `Stop`                | When Claude finishes responding                                                                                                                        |
| `StopFailure`         | When the turn ends due to an API error. Output and exit code are ignored                                                                               |
| `TeammateIdle`        | When an agent team teammate is about to go idle                                                                                                        |
| `InstructionsLoaded`  | When a CLAUDE.md or `.claude/rules/*.md` file is loaded into context. Fires at session start and when files are lazily loaded during a session         |
| `ConfigChange`        | When a configuration file changes during a session                                                                                                     |
| `CwdChanged`          | When the working directory changes, for example when Claude executes a `cd` command. Useful for reactive environment management with tools like direnv |
| `FileChanged`         | When a watched file changes on disk. The `matcher` field specifies which filenames to watch                                                            |
| `WorktreeCreate`      | When a worktree is being created via `--worktree` or `isolation: "worktree"`. Replaces default git behavior                                            |
| `WorktreeRemove`      | When a worktree is being removed, either at session exit or when a subagent finishes                                                                   |
| `PreCompact`          | Before context compaction                                                                                                                              |
| `PostCompact`         | After context compaction completes                                                                                                                     |
| `Elicitation`         | When an MCP server requests user input during a tool call                                                                                              |
| `ElicitationResult`   | After a user responds to an MCP elicitation, before the response is sent back to the server                                                            |
| `SessionEnd`          | When a session terminates                                                                                                                              |

| 事件                  | 触发时机                                                                                                                                               |
| :-------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| `SessionStart`        | 会话开始或恢复时                                                                                                                                       |
| `Setup`               | 使用 `--init-only` 启动 Claude Code，或在 `-p` 模式下使用 `--init` 或 `--maintenance` 时。用于 CI（持续集成）或脚本中的一次性准备工作                  |
| `UserPromptSubmit`    | 你提交 prompt 时，在 Claude 处理之前                                                                                                                   |
| `UserPromptExpansion` | 用户输入的命令展开为 prompt 时，在到达 Claude 之前。可以阻止展开                                                                                        |
| `PreToolUse`          | 工具调用执行之前。可以阻止执行                                                                                                                          |
| `PermissionRequest`   | 权限对话框出现时                                                                                                                                       |
| `PermissionDenied`    | 工具调用被自动模式分类器拒绝时。返回 `{retry: true}` 告知模型可以重试被拒绝的工具调用                                                                  |
| `PostToolUse`         | 工具调用成功之后                                                                                                                                       |
| `PostToolUseFailure`  | 工具调用失败之后                                                                                                                                       |
| `PostToolBatch`       | 一整批并行工具调用完成后，下一次模型调用之前                                                                                                            |
| `Notification`        | Claude Code 发送通知时                                                                                                                                 |
| `MessageDisplay`      | 助手消息文本显示期间                                                                                                                                   |
| `SubagentStart`       | subagent 被创建时                                                                                                                                     |
| `SubagentStop`        | subagent 完成时                                                                                                                                       |
| `TaskCreated`         | 通过 `TaskCreate` 创建任务时                                                                                                                           |
| `TaskCompleted`       | 任务被标记为完成时                                                                                                                                     |
| `Stop`                | Claude 完成响应时                                                                                                                                      |
| `StopFailure`         | 轮次因 API 错误结束时。输出和退出码被忽略                                                                                                               |
| `TeammateIdle`        | agent team（智能体团队）中的队友即将空闲时                                                                                                              |
| `InstructionsLoaded`  | CLAUDE.md 或 `.claude/rules/*.md` 文件被加载到上下文时。在会话启动时以及会话期间延迟加载文件时触发                                                      |
| `ConfigChange`        | 会话期间配置文件变更时                                                                                                                                 |
| `CwdChanged`          | 工作目录变更时，例如 Claude 执行 `cd` 命令时。适用于配合 direnv 等工具进行响应式环境管理                                                                |
| `FileChanged`         | 被监视的文件在磁盘上变更时。`matcher` 字段指定要监视哪些文件名                                                                                          |
| `WorktreeCreate`      | 通过 `--worktree` 或 `isolation: "worktree"` 创建 worktree 时。替代默认的 git 行为                                                                     |
| `WorktreeRemove`      | 移除 worktree 时，无论是在会话退出还是 subagent 完成时                                                                                                 |
| `PreCompact`          | 上下文压缩之前                                                                                                                                         |
| `PostCompact`         | 上下文压缩完成之后                                                                                                                                     |
| `Elicitation`         | MCP 服务器在工具调用期间请求用户输入时                                                                                                                  |
| `ElicitationResult`   | 用户响应 MCP elicitation 后，响应被发送回服务器之前                                                                                                     |
| `SessionEnd`          | 会话终止时                                                                                                                                             |

Each hook has a `type` that determines how it runs. Most hooks use `"type": "command"`, which runs a shell command. Four other types are available:

每个 hook 都有一个 `type` 决定其运行方式。大多数 hook 使用 `"type": "command"`，即运行一条 shell 命令。另外还有四种类型可用：

- `"type": "http"`: POST event data to a URL. See HTTP hooks.
- `"type": "mcp_tool"`: call a tool on an already-connected MCP server. See MCP tool hooks.
- `"type": "prompt"`: single-turn LLM evaluation. See Prompt-based hooks.
- `"type": "agent"`: multi-turn verification with tool access. Agent hooks are experimental and may change. See Agent-based hooks.

- `"type": "http"`：将事件数据 POST 到一个 URL。参阅 HTTP hooks。
- `"type": "mcp_tool"`：在已连接的 MCP 服务器上调用工具。参阅 MCP 工具 hooks。
- `"type": "prompt"`：单轮 LLM 评估。参阅基于 prompt 的 hooks。
- `"type": "agent"`：带工具访问的多轮验证。agent hook 是实验性功能，可能发生变化。参阅基于 agent 的 hooks。

### Combine results from multiple hooks

When multiple hooks match the same event, every hook's command runs to completion before Claude Code merges the results. One hook returning `deny` doesn't stop sibling hooks from executing. Don't rely on one hook's `deny` to suppress side effects in another hook.

当多个 hook 匹配同一事件时，每个 hook 的命令都会运行完毕，然后 Claude Code 才合并结果。一个 hook 返回 `deny` 不会阻止同级 hook 的执行。不要依赖一个 hook 的 `deny` 来抑制另一个 hook 的副作用。

After all matching hooks finish, Claude Code combines their outputs. For `PreToolUse` permission decisions, the most restrictive answer applies, in the order `deny`, `defer`, `ask`, `allow`. Text from `additionalContext` is kept from every hook and passed to Claude together.

所有匹配的 hook 完成后，Claude Code 合并它们的输出。对于 `PreToolUse` 权限决策，采用最严格的回答，优先级顺序为 `deny`、`defer`、`ask`、`allow`。来自 `additionalContext` 的文本会从每个 hook 中保留并一起传递给 Claude。

The example below registers two `PreToolUse` hooks on `Bash`. The first appends every command to a log file and exits 0. The second runs a script that exits 2 to deny when the command contains `rm -rf`:

以下示例在 `Bash` 上注册了两个 `PreToolUse` hook。第一个将每条命令追加到日志文件并以 0 退出。第二个运行一个脚本，当命令包含 `rm -rf` 时以 2 退出以拒绝：

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "jq -r .tool_input.command >> ~/.claude/bash.log"
          },
          {
            "type": "command",
            "command": "\"$CLAUDE_PROJECT_DIR\"/.claude/hooks/block-rm-rf.sh"
          }
        ]
      }
    ]
  }
}
```

When Claude tries to run `rm -rf /tmp/build`, both hooks execute in parallel. The logging hook writes the command to `~/.claude/bash.log` and exits 0, which reports no decision. The guardrail hook exits 2, which denies the tool call. The deny takes precedence, so Claude Code blocks the command and shows Claude the guardrail's stderr. The log entry is still written because the logging hook already ran.

当 Claude 尝试运行 `rm -rf /tmp/build` 时，两个 hook 并行执行。日志 hook 将命令写入 `~/.claude/bash.log` 并以 0 退出，表示不报告任何决策。护栏 hook 以 2 退出，拒绝该工具调用。deny 优先级更高，因此 Claude Code 阻止该命令并向 Claude 显示护栏 hook 的 stderr。日志条目仍然被写入，因为日志 hook 已经运行完毕。

### Read input and return output

Hooks communicate with Claude Code through stdin, stdout, stderr, and exit codes. When an event fires, Claude Code passes event-specific data as JSON to your script's stdin. Your script reads that data, does its work, and tells Claude Code what to do next via the exit code.

hook 通过 stdin、stdout、stderr 和退出码与 Claude Code 通信。当事件触发时，Claude Code 将事件特定的数据以 JSON 格式传递到你脚本的 stdin。你的脚本读取该数据、执行操作，然后通过退出码告知 Claude Code 接下来做什么。

#### Hook input

Every event includes common fields like `session_id` and `cwd`, but each event type adds different data. For example, when Claude runs a Bash command, a `PreToolUse` hook receives something like this on stdin:

每个事件都包含 `session_id` 和 `cwd` 等公共字段，但每种事件类型会添加不同的数据。例如，当 Claude 运行 Bash 命令时，`PreToolUse` hook 会在 stdin 上收到类似以下内容：

```json
{
  "session_id": "abc123",          // unique ID for this session
  "cwd": "/Users/sarah/myproject", // working directory when the event fired
  "hook_event_name": "PreToolUse", // which event triggered this hook
  "tool_name": "Bash",             // the tool Claude is about to use
  "tool_input": {                  // the arguments Claude passed to the tool
    "command": "npm test"          // for Bash, this is the shell command
  }
}
```

Your script can parse that JSON and act on any of those fields. `UserPromptSubmit` hooks get the `prompt` text instead, `SessionStart` hooks get a `source` of `startup`, `resume`, `clear`, or `compact`, and so on. See Common input fields in the reference for shared fields, and each event's section for event-specific schemas.

你的脚本可以解析该 JSON 并对其中的任何字段进行操作。`UserPromptSubmit` hook 改为获取 `prompt` 文本，`SessionStart` hook 获取 `source` 值为 `startup`、`resume`、`clear` 或 `compact`，依此类推。公共字段请参阅参考文档中的"公共输入字段"，各事件的特定 schema 请参阅对应事件章节。

#### Hook output

Your script tells Claude Code what to do next by writing to stdout or stderr and exiting with a specific code. The following `PreToolUse` hook blocks a command:

你的脚本通过写入 stdout 或 stderr 并以特定退出码退出，来告知 Claude Code 接下来做什么。以下 `PreToolUse` hook 会阻止一条命令：

```bash
#!/bin/bash
INPUT=$(cat)
COMMAND=$(echo "$INPUT" | jq -r '.tool_input.command')

if echo "$COMMAND" | grep -q "drop table"; then
  echo "Blocked: dropping tables is not allowed" >&2  # stderr becomes Claude's feedback
  exit 2                                               # exit 2 = block the action
fi

exit 0  # exit 0 = no decision; the normal permission flow applies
```

The exit code determines what happens next:

退出码决定接下来发生什么：

- **Exit 0**: the hook reports no objection and the action proceeds normally. For a `PreToolUse` hook this doesn't approve the tool call: the normal permission flow still applies. For `UserPromptSubmit`, `UserPromptExpansion`, and `SessionStart` hooks, anything you write to stdout is added to Claude's context.

- **Exit 2**: the action is blocked. Write a reason to stderr, and Claude receives it as feedback so it can adjust. Some events can't be blocked: for `SessionStart`, `Setup`, `Notification`, and others, exit 2 shows stderr to the user and execution continues. See exit code 2 behavior per event for the full list.

- **Any other exit code**: the action proceeds. The transcript shows a `<hook name> hook error` notice followed by the first line of stderr; the full stderr goes to the debug log.

- **退出码 0**：hook 报告无异议，操作正常继续。对于 `PreToolUse` hook，这并不批准工具调用：正常的权限流程仍然适用。对于 `UserPromptSubmit`、`UserPromptExpansion` 和 `SessionStart` hook，你写入 stdout 的任何内容都会被添加到 Claude 的上下文中。

- **退出码 2**：操作被阻止。将原因写入 stderr，Claude 会将其作为反馈接收以便调整。某些事件无法被阻止：对于 `SessionStart`、`Setup`、`Notification` 等，退出码 2 会向用户显示 stderr 并继续执行。完整列表请参阅"各事件的退出码 2 行为"。

- **任何其他退出码**：操作继续进行。记录中会显示 `<hook name> hook error` 通知，后跟 stderr 的第一行；完整的 stderr 会写入 debug 日志。

#### Structured JSON output

Exit codes only let you block or stay silent. For more control, exit 0 and print a JSON object to stdout instead.

退出码只能让你阻止操作或保持沉默。如需更多控制，请以退出码 0 退出并向 stdout 打印一个 JSON 对象。

<Note>
Use exit 2 to block with a stderr message, or exit 0 with JSON for structured control. Don't mix them: Claude Code ignores JSON when you exit 2.
</Note>

<注意>
使用退出码 2 配合 stderr 消息来阻止，或使用退出码 0 配合 JSON 进行结构化控制。不要混用：当你以退出码 2 退出时，Claude Code 会忽略 JSON。
</注意>

For example, a `PreToolUse` hook can deny a tool call and tell Claude why, or escalate it to the user for approval:

例如，`PreToolUse` hook 可以拒绝工具调用并告知 Claude 原因，或将其升级为用户审批：

```json
{
  "hookSpecificOutput": {
    "hookEventName": "PreToolUse",
    "permissionDecision": "deny",
    "permissionDecisionReason": "Use rg instead of grep for better performance"
  }
}
```

With `"deny"`, Claude Code cancels the tool call and feeds `permissionDecisionReason` back to Claude. These `permissionDecision` values are specific to `PreToolUse`:

使用 `"deny"` 时，Claude Code 取消工具调用并将 `permissionDecisionReason` 反馈给 Claude。这些 `permissionDecision` 值是 `PreToolUse` 特有的：

- `"allow"`: skip the interactive permission prompt. Deny and ask rules, including enterprise managed deny lists, still apply
- `"deny"`: cancel the tool call and send the reason to Claude
- `"ask"`: show the permission prompt to the user as normal

- `"allow"`：跳过交互式权限提示。deny 和 ask 规则（包括企业管理的 deny 列表）仍然生效
- `"deny"`：取消工具调用并将原因发送给 Claude
- `"ask"`：正常向用户显示权限提示

A fourth value, `"defer"`, is available in non-interactive mode with the `-p` flag. It exits the process with the tool call preserved so an Agent SDK wrapper can collect input and resume. See Defer a tool call for later in the reference.

第四个值 `"defer"` 在使用 `-p` 标志的非交互模式下可用。它会退出进程但保留工具调用，以便 Agent SDK 包装器收集输入后恢复。参阅参考文档中的"延迟工具调用"。

Returning `"allow"` skips the interactive prompt but doesn't override permission rules. If a deny rule matches the tool call, the call is blocked even when your hook returns `"allow"`. If an ask rule matches, the user is still prompted. This means deny rules from any settings scope, including managed settings, always take precedence over hook approvals.

返回 `"allow"` 会跳过交互式提示，但不会覆盖权限规则。如果 deny 规则匹配该工具调用，即使你的 hook 返回 `"allow"`，调用也会被阻止。如果 ask 规则匹配，用户仍会被提示。这意味着来自任何 settings 作用域的 deny 规则（包括托管 settings）始终优先于 hook 批准。

Other events use different decision patterns. For example, `PostToolUse` and `Stop` hooks use a top-level `decision: "block"` field, while `PermissionRequest` uses `hookSpecificOutput.decision.behavior`. See the summary table in the reference for a full breakdown by event.

其他事件使用不同的决策模式。例如，`PostToolUse` 和 `Stop` hook 使用顶层的 `decision: "block"` 字段，而 `PermissionRequest` 使用 `hookSpecificOutput.decision.behavior`。完整的事件分类请参阅参考文档中的摘要表。

For `UserPromptSubmit` hooks, use `additionalContext` instead to inject text into Claude's context.

对于 `UserPromptSubmit` hook，请改用 `additionalContext` 来向 Claude 的上下文注入文本。

Hooks with `type: "prompt"` handle output differently: see Prompt-based hooks.

`type: "prompt"` 的 hook 以不同方式处理输出：参阅"基于 prompt 的 hooks"。

### Filter hooks with matchers

Without a matcher, a hook fires on every occurrence of its event. Matchers let you narrow that down. For example, if you want to run a formatter only after file edits, not after every tool call, add a matcher to your `PostToolUse` hook:

没有 matcher 时，hook 会在其事件的每次出现时触发。matcher 允许你缩小范围。例如，如果你只想在文件编辑后运行格式化工具而非每次工具调用后，可以为 `PostToolUse` hook 添加 matcher：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Edit|Write",
        "hooks": [
          { "type": "command", "command": "prettier --write ..." }
        ]
      }
    ]
  }
}
```

The `"Edit|Write"` matcher fires only when Claude uses the `Edit` or `Write` tool, not when it uses `Bash`, `Read`, or any other tool. See Matcher patterns for how plain names and regular expressions are evaluated.

`"Edit|Write"` matcher 仅在 Claude 使用 `Edit` 或 `Write` 工具时触发，而非使用 `Bash`、`Read` 或任何其他工具时。关于纯名称和正则表达式的求值方式，请参阅"Matcher 模式"。

<Note>
Claude can also create or modify files by running shell commands through the `Bash` tool. If your hook must see every file change, such as for compliance scanning or audit logging, add a `Stop` hook that scans the working tree once per turn. For per-call coverage instead, also match `Bash` and have your script list modified and untracked files with `git status --porcelain`.
</Note>

<注意>
Claude 也可以通过 `Bash` 工具运行 shell 命令来创建或修改文件。如果你的 hook 必须看到每次文件变更（例如用于合规扫描或审计日志），请添加一个 `Stop` hook，每轮扫描一次工作树。若需按调用覆盖，还应匹配 `Bash` 并让你的脚本用 `git status --porcelain` 列出已修改和未跟踪的文件。
</注意>

Each event type matches on a specific field:

每种事件类型匹配特定的字段：

| Event                                                                                                                                                           | What the matcher filters                                              | Example matcher values                                                                                                                                                              |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PreToolUse`, `PostToolUse`, `PostToolUseFailure`, `PermissionRequest`, `PermissionDenied`                                                                      | tool name                                                             | `Bash`, `Edit\|Write`, `mcp__.*`                                                                                                                                                    |
| `SessionStart`                                                                                                                                                  | how the session started                                               | `startup`, `resume`, `clear`, `compact`                                                                                                                                             |
| `Setup`                                                                                                                                                         | which CLI flag triggered setup                                        | `init`, `maintenance`                                                                                                                                                               |
| `SessionEnd`                                                                                                                                                    | why the session ended                                                 | `clear`, `resume`, `logout`, `prompt_input_exit`, `bypass_permissions_disabled`, `other`                                                                                            |
| `Notification`                                                                                                                                                  | notification type                                                     | `permission_prompt`, `idle_prompt`, `auth_success`, `elicitation_dialog`, `elicitation_complete`, `elicitation_response`, `agent_needs_input`, `agent_completed`                    |
| `SubagentStart`                                                                                                                                                 | agent type                                                            | `general-purpose`, `Explore`, `Plan`, or custom agent names                                                                                                                         |
| `PreCompact`, `PostCompact`                                                                                                                                     | what triggered compaction                                             | `manual`, `auto`                                                                                                                                                                    |
| `SubagentStop`                                                                                                                                                  | agent type                                                            | same values as `SubagentStart`                                                                                                                                                      |
| `ConfigChange`                                                                                                                                                  | configuration source                                                  | `user_settings`, `project_settings`, `local_settings`, `policy_settings`, `skills`                                                                                                  |
| `StopFailure`                                                                                                                                                   | error type                                                            | `rate_limit`, `overloaded`, `authentication_failed`, `oauth_org_not_allowed`, `billing_error`, `invalid_request`, `model_not_found`, `server_error`, `max_output_tokens`, `unknown` |

| 事件                                                                                                                                                            | matcher 过滤的内容                                                    | 示例 matcher 值                                                                                                                                                                      |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `PreToolUse`、`PostToolUse`、`PostToolUseFailure`、`PermissionRequest`、`PermissionDenied`                                                                      | 工具名                                                                | `Bash`, `Edit\|Write`, `mcp__.*`                                                                                                                                                    |
| `SessionStart`                                                                                                                                                  | 会话启动方式                                                           | `startup`, `resume`, `clear`, `compact`                                                                                                                                             |
| `Setup`                                                                                                                                                         | 触发 setup 的 CLI 标志                                                | `init`, `maintenance`                                                                                                                                                               |
| `SessionEnd`                                                                                                                                                    | 会话结束原因                                                           | `clear`, `resume`, `logout`, `prompt_input_exit`, `bypass_permissions_disabled`, `other`                                                                                            |
| `Notification`                                                                                                                                                  | 通知类型                                                               | `permission_prompt`, `idle_prompt`, `auth_success`, `elicitation_dialog`, `elicitation_complete`, `elicitation_response`, `agent_needs_input`, `agent_completed`                    |
| `SubagentStart`                                                                                                                                                 | agent 类型                                                             | `general-purpose`, `Explore`, `Plan` 或自定义 agent 名称                                                                                                                             |
| `PreCompact`、`PostCompact`                                                                                                                                     | 触发压缩的原因                                                         | `manual`, `auto`                                                                                                                                                                    |
| `SubagentStop`                                                                                                                                                  | agent 类型                                                             | 与 `SubagentStart` 相同的值                                                                                                                                                          |
| `ConfigChange`                                                                                                                                                  | 配置来源                                                               | `user_settings`, `project_settings`, `local_settings`, `policy_settings`, `skills`                                                                                                  |
| `StopFailure`                                                                                                                                                   | 错误类型                                                               | `rate_limit`, `overloaded`, `authentication_failed`, `oauth_org_not_allowed`, `billing_error`, `invalid_request`, `model_not_found`, `server_error`, `max_output_tokens`, `unknown` |

### Configure hook location

Where you add a hook determines its scope:

hook 添加的位置决定了其作用域：

| Location                        | Scope                   | Shareable                         |
| :------------------------------ | :---------------------- | :-------------------------------- |
| `~/.claude/settings.json`       | All your projects       | No, local to your machine         |
| `.claude/settings.json`         | Single project          | Yes, can be committed to the repo |
| `.claude/settings.local.json`   | Single project          | No, gitignored when Claude Code creates it |
| Managed policy settings         | Organization-wide       | Yes, admin-controlled             |
| Plugin `hooks/hooks.json`       | When plugin is enabled  | Yes, bundled with the plugin      |
| Skill or agent frontmatter      | While the skill or agent is active | Yes, defined in the component file |

| 位置                            | 作用域                  | 可共享                             |
| :------------------------------ | :---------------------- | :-------------------------------- |
| `~/.claude/settings.json`       | 你的所有项目            | 否，仅限本机                       |
| `.claude/settings.json`         | 单个项目                | 是，可以提交到仓库                 |
| `.claude/settings.local.json`   | 单个项目                | 否，Claude Code 创建时会被 gitignore |
| 托管策略 settings               | 全组织范围              | 是，由管理员控制                   |
| Plugin `hooks/hooks.json`       | plugin 启用时           | 是，随 plugin 打包                 |
| Skill 或 agent frontmatter      | skill 或 agent 活动期间 | 是，定义在组件文件中               |

Run `/hooks` in Claude Code to browse all configured hooks grouped by event.

在 Claude Code 中运行 `/hooks` 可以按事件浏览所有已配置的 hook。

To disable hooks, set `"disableAllHooks": true` in your settings file. Hooks configured in managed settings still run unless `disableAllHooks` is also set there.

要禁用 hook，在 settings 文件中设置 `"disableAllHooks": true`。托管 settings 中配置的 hook 仍然会运行，除非在那里也设置了 `disableAllHooks`。

If you edit settings files directly while Claude Code is running, the file watcher normally picks up hook changes automatically.

如果你在 Claude Code 运行期间直接编辑 settings 文件，文件监视器通常会自动获取 hook 变更。

### Prompt-based hooks

For decisions that require judgment rather than deterministic rules, use `type: "prompt"` hooks. Instead of running a shell command, Claude Code sends your prompt and the hook's input data to a Claude model, Haiku by default, to make the decision. You can specify a different model with the `model` field if you need more capability.

对于需要判断力而非确定性规则的决策，使用 `type: "prompt"` hook。Claude Code 不会运行 shell 命令，而是将你的 prompt 和 hook 的输入数据发送给 Claude 模型（默认为 Haiku）来做决策。如果需要更强的能力，可以通过 `model` 字段指定不同的模型。

The model's only job is to return a yes/no decision as JSON:

模型的唯一任务是返回一个 JSON 格式的是/否决策：

- `"ok": true`: the action proceeds
- `"ok": false`: what happens depends on the event:
  - `Stop` and `SubagentStop`: the `reason` is fed back to Claude so it keeps working
  - `PreToolUse`: the tool call is denied and the `reason` is returned to Claude as the tool error, so it can adjust and continue
  - `PostToolUse`, `PostToolBatch`, `UserPromptSubmit`, and `UserPromptExpansion`: the turn ends and the `reason` appears in the chat as a warning line

- `"ok": true`：操作继续
- `"ok": false`：具体行为取决于事件：
  - `Stop` 和 `SubagentStop`：`reason` 被反馈给 Claude 使其继续工作
  - `PreToolUse`：工具调用被拒绝，`reason` 作为工具错误返回给 Claude，以便其调整后继续
  - `PostToolUse`、`PostToolBatch`、`UserPromptSubmit` 和 `UserPromptExpansion`：轮次结束，`reason` 在聊天中作为警告行显示

This example uses a `Stop` hook to ask the model whether all requested tasks are complete. If the model returns `"ok": false`, Claude keeps working and uses the `reason` as its next instruction:

此示例使用 `Stop` hook 询问模型是否所有请求的任务都已完成。如果模型返回 `"ok": false`，Claude 会继续工作并将 `reason` 用作下一条指令：

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "prompt",
            "prompt": "Check if all tasks are complete. If not, respond with {\"ok\": false, \"reason\": \"what remains to be done\"}."
          }
        ]
      }
    ]
  }
}
```

For full configuration options, see Prompt-based hooks in the reference.

完整的配置选项请参阅参考文档中的"基于 prompt 的 hooks"。

### Agent-based hooks

Agent hooks are experimental. Behavior and configuration may change in future releases. For production workflows, prefer command hooks.

Agent hook 是实验性功能。行为和配置可能在未来版本中变化。对于生产 workflow（工作流），建议使用 command hook。

When verification requires inspecting files or running commands, use `type: "agent"` hooks. Unlike prompt hooks, which make a single LLM call, agent hooks spawn a subagent that can read files, search code, and use other tools to verify conditions before returning a decision.

当验证需要检查文件或运行命令时，使用 `type: "agent"` hook。与仅进行单次 LLM 调用的 prompt hook 不同，agent hook 会创建一个 subagent，它可以读取文件、搜索代码并使用其他工具来验证条件，然后返回决策。

Agent hooks use the same `"ok"` / `"reason"` response format as prompt hooks, but with a longer default timeout of 60 seconds and up to 50 tool-use turns.

Agent hook 使用与 prompt hook 相同的 `"ok"` / `"reason"` 响应格式，但默认超时更长，为 60 秒，最多 50 轮工具使用。

This example verifies that tests pass before allowing Claude to stop:

此示例验证测试是否通过，然后才允许 Claude 停止：

```json
{
  "hooks": {
    "Stop": [
      {
        "hooks": [
          {
            "type": "agent",
            "prompt": "Verify that all unit tests pass. Run the test suite and check the results. $ARGUMENTS",
            "timeout": 120
          }
        ]
      }
    ]
  }
}
```

Use prompt hooks when the hook input data alone is enough to make a decision. Use agent hooks when you need to verify something against the actual state of the codebase.

当 hook 输入数据本身足以做出决策时，使用 prompt hook。当你需要根据代码库的实际状态验证某些内容时，使用 agent hook。

For full configuration options, see Agent-based hooks in the reference.

完整的配置选项请参阅参考文档中的"基于 agent 的 hooks"。

### HTTP hooks

Use `type: "http"` hooks to POST event data to an HTTP endpoint instead of running a shell command. The endpoint receives the same JSON that a command hook would receive on stdin, and returns results through the HTTP response body using the same JSON format.

使用 `type: "http"` hook 将事件数据 POST 到 HTTP 端点，而非运行 shell 命令。端点接收与 command hook 在 stdin 上接收的相同 JSON，并通过 HTTP 响应体使用相同的 JSON 格式返回结果。

HTTP hooks are useful when you want a web server, cloud function, or external service to handle hook logic: for example, a shared audit service that logs tool use events across a team.

当你希望 Web 服务器、云函数或外部服务来处理 hook 逻辑时，HTTP hook 非常有用：例如，一个记录整个团队工具使用事件的共享审计服务。

This example posts every tool use to a local logging service:

此示例将每次工具使用 POST 到本地日志服务：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "hooks": [
          {
            "type": "http",
            "url": "http://localhost:8080/hooks/tool-use",
            "headers": {
              "Authorization": "Bearer $MY_TOKEN"
            },
            "allowedEnvVars": ["MY_TOKEN"]
          }
        ]
      }
    ]
  }
}
```

The endpoint should return a JSON response body using the same output format as command hooks. To block a tool call, return a 2xx response with the appropriate `hookSpecificOutput` fields. HTTP status codes alone can't block actions.

端点应返回使用与 command hook 相同输出格式的 JSON 响应体。要阻止工具调用，返回一个带有相应 `hookSpecificOutput` 字段的 2xx 响应。仅靠 HTTP 状态码无法阻止操作。

Header values support environment variable interpolation using `$VAR_NAME` or `${VAR_NAME}` syntax. Only variables listed in the `allowedEnvVars` array are resolved; all other `$VAR` references remain empty.

Header 值支持使用 `$VAR_NAME` 或 `${VAR_NAME}` 语法的环境变量插值。只有 `allowedEnvVars` 数组中列出的变量会被解析；所有其他 `$VAR` 引用保持为空。

For full configuration options and response handling, see HTTP hooks in the reference.

完整的配置选项和响应处理请参阅参考文档中的"HTTP hooks"。

### Limitations and troubleshooting

### 限制与故障排除

#### Limitations

Keep these constraints in mind when designing hooks:

设计 hook 时请注意以下约束：

- Command hooks communicate through stdout, stderr, and exit codes only. They can't trigger `/` commands or tool calls. Text returned via `additionalContext` is injected as a system reminder that Claude reads as plain text. HTTP hooks communicate through the response body instead.

- Hook timeouts vary by type. Override per hook with the `timeout` field in seconds.
  - `command`, `http`, `mcp_tool`: 10 minutes. `UserPromptSubmit` lowers these to 30 seconds, and `MessageDisplay` lowers them to 10 seconds.
  - `prompt`: 30 seconds.
  - `agent`: 60 seconds.

- `PostToolUse` hooks can't undo actions since the tool has already executed.

- `PermissionRequest` hooks don't fire in non-interactive mode with the `-p` flag. Use `PreToolUse` hooks for automated permission decisions.

- `Stop` hooks fire whenever Claude finishes responding, not only at task completion. They don't fire on user interrupts. API errors fire `StopFailure` instead.

- When multiple `PreToolUse` hooks return `updatedInput` to rewrite a tool's arguments, the last one to finish takes effect. Since hooks run in parallel, the order is non-deterministic. Avoid having more than one hook modify the same tool's input.

- Command hook 仅通过 stdout、stderr 和退出码通信。它们无法触发 `/` 命令或工具调用。通过 `additionalContext` 返回的文本会作为系统提醒注入，Claude 将其作为纯文本读取。HTTP hook 改为通过响应体通信。

- Hook 超时因类型而异。可通过 `timeout` 字段以秒为单位逐个覆盖。
  - `command`、`http`、`mcp_tool`：10 分钟。`UserPromptSubmit` 将其降低为 30 秒，`MessageDisplay` 降低为 10 秒。
  - `prompt`：30 秒。
  - `agent`：60 秒。

- `PostToolUse` hook 无法撤销操作，因为工具已经执行完毕。

- `PermissionRequest` hook 在使用 `-p` 标志的非交互模式下不会触发。如需自动化权限决策，请使用 `PreToolUse` hook。

- `Stop` hook 在 Claude 每次完成响应时触发，而非仅在任务完成时。它们不会在用户中断时触发。API 错误改为触发 `StopFailure`。

- 当多个 `PreToolUse` hook 返回 `updatedInput` 来重写工具参数时，最后一个完成的生效。由于 hook 并行运行，顺序是不确定的。避免让多个 hook 修改同一工具的输入。

#### Hooks and permission modes

`PreToolUse` hooks fire before any permission-mode check. A hook that returns `permissionDecision: "deny"` blocks the tool even in `bypassPermissions` mode or with `--dangerously-skip-permissions`. This lets you enforce policy that users can't bypass by changing their permission mode.

`PreToolUse` hook 在任何权限模式检查之前触发。返回 `permissionDecision: "deny"` 的 hook 即使在 `bypassPermissions` 模式或使用 `--dangerously-skip-permissions` 时也会阻止工具。这让你可以强制执行用户无法通过更改权限模式来绕过的策略。

The reverse is not true: a hook returning `"allow"` doesn't bypass deny rules from settings. Hooks can tighten restrictions but not loosen them past what permission rules allow.

反过来则不成立：返回 `"allow"` 的 hook 不会绕过 settings 中的 deny 规则。hook 可以收紧限制，但不能放松到超出权限规则允许的范围。

#### Hook not firing

The hook is configured but never executes.

hook 已配置但从不执行。

- Run `/hooks` and confirm the hook appears under the correct event
- Check that the matcher pattern matches the tool name exactly. Matchers are case-sensitive
- Verify you're triggering the right event type: `PreToolUse` fires before tool execution, `PostToolUse` fires after
- If using `PermissionRequest` hooks in non-interactive mode with the `-p` flag, switch to `PreToolUse` instead

- 运行 `/hooks` 确认 hook 出现在正确的事件下
- 检查 matcher 模式是否与工具名完全匹配。matcher 区分大小写
- 确认你触发的是正确的事件类型：`PreToolUse` 在工具执行前触发，`PostToolUse` 在之后触发
- 如果在非交互模式下使用 `-p` 标志的 `PermissionRequest` hook，请改用 `PreToolUse`

#### Hook error in output

You see a message like "PreToolUse hook error:" in the transcript.

你在记录中看到类似 "PreToolUse hook error:" 的消息。

Your script exited with a non-zero code unexpectedly. Test it manually by piping sample JSON:

你的脚本意外地以非零码退出。通过管道传入示例 JSON 来手动测试：

```bash
echo '{"tool_name":"Bash","tool_input":{"command":"ls"}}' | ./my-hook.sh
echo $?  # Check the exit code
```

- If you see "command not found", use absolute paths or `${CLAUDE_PROJECT_DIR}` to reference scripts. To avoid shell quoting entirely, add `"args": []` to switch to exec form, which spawns the script directly without a shell
- If you see "jq: command not found", install `jq` or use Python/Node.js for JSON parsing
- If the script isn't running at all, make it executable: `chmod +x ./my-hook.sh`

- 如果看到 "command not found"，请使用绝对路径或 `${CLAUDE_PROJECT_DIR}` 来引用脚本。要完全避免 shell 引号问题，添加 `"args": []` 切换到 exec 形式，直接生成脚本而不经过 shell
- 如果看到 "jq: command not found"，请安装 `jq` 或使用 Python/Node.js 进行 JSON 解析
- 如果脚本根本没有运行，请赋予可执行权限：`chmod +x ./my-hook.sh`

#### /hooks shows no hooks configured

You edited a settings file but the hooks don't appear in the menu.

你编辑了 settings 文件但 hook 没有出现在菜单中。

- File edits are normally picked up automatically. If they haven't appeared after a few seconds, the file watcher may have missed the change: restart your session to force a reload.
- Verify your JSON is valid: trailing commas and comments aren't allowed
- Confirm the settings file is in the correct location: `.claude/settings.json` for project hooks, `~/.claude/settings.json` for global hooks

- 文件编辑通常会被自动获取。如果几秒后仍未出现，文件监视器可能遗漏了变更：重启会话以强制重新加载。
- 验证你的 JSON 是否有效：不允许尾逗号和注释
- 确认 settings 文件位于正确位置：项目 hook 用 `.claude/settings.json`，全局 hook 用 `~/.claude/settings.json`

#### Stop hook hits the block cap

Claude keeps working instead of stopping, then ends the turn with a warning that the Stop hook blocked too many consecutive times.

Claude 持续工作而非停止，然后以警告结束轮次，提示 Stop hook 连续阻止次数过多。

Claude Code overrides a Stop hook after it blocks eight times in a row without progress. Your hook script needs to check whether it already triggered a continuation. Parse the `stop_hook_active` field from the JSON input and exit early if it's `true`:

Claude Code 在 Stop hook 连续阻止八次且无进展后会覆盖该 hook。你的 hook 脚本需要检查是否已经触发过续接。从 JSON 输入中解析 `stop_hook_active` 字段，如果为 `true` 则提前退出：

```bash
#!/bin/bash
INPUT=$(cat)
if [ "$(echo "$INPUT" | jq -r '.stop_hook_active')" = "true" ]; then
  exit 0  # Allow Claude to stop
fi
# ... rest of your hook logic
```

If your hook legitimately needs more than eight iterations to converge, raise the cap with `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`.

如果你的 hook 确实需要超过八次迭代才能收敛，可以通过 `CLAUDE_CODE_STOP_HOOK_BLOCK_CAP` 提高上限。

#### JSON validation failed

Claude Code shows a JSON parsing error even though your hook script outputs valid JSON.

Claude Code 显示 JSON 解析错误，尽管你的 hook 脚本输出了有效的 JSON。

When Claude Code runs a shell-form command hook, one without `args`, it spawns `sh -c` on macOS and Linux or Git Bash on Windows by default. This shell is non-interactive, but Git Bash and some configurations, such as `BASH_ENV` pointing at `~/.bashrc`, still source your profile. If that profile contains unconditional `echo` statements, the output gets prepended to your hook's JSON:

当 Claude Code 运行 shell 形式的 command hook（没有 `args`）时，默认在 macOS 和 Linux 上生成 `sh -c`，在 Windows 上生成 Git Bash。此 shell 是非交互式的，但 Git Bash 和某些配置（如 `BASH_ENV` 指向 `~/.bashrc`）仍会加载你的 profile。如果该 profile 包含无条件的 `echo` 语句，输出会被前置到你的 hook JSON 之前：

```
Shell ready on arm64
{"decision": "block", "reason": "Not allowed"}
```

Claude Code tries to parse this as JSON and fails. To fix this, wrap echo statements in your shell profile so they only run in interactive shells:

Claude Code 尝试将此解析为 JSON 并失败。要修复此问题，请将 shell profile 中的 echo 语句包裹起来，使其仅在交互式 shell 中运行：

```bash
# In ~/.zshrc or ~/.bashrc
if [[ $- == *i* ]]; then
  echo "Shell ready"
fi
```

The `$-` variable contains shell flags, and `i` means interactive. Hooks run in non-interactive shells, so the echo is skipped.

`$-` 变量包含 shell 标志，`i` 表示交互式。hook 在非交互式 shell 中运行，因此 echo 被跳过。

#### Debug techniques

The transcript view, toggled with `Ctrl+O`, shows a one-line summary for each hook that fired: success is silent, blocking errors show stderr, and non-blocking errors show a `<hook name> hook error` notice followed by the first line of stderr.

记录视图（用 `Ctrl+O` 切换）为每个触发的 hook 显示一行摘要：成功时静默，阻止性错误显示 stderr，非阻止性错误显示 `<hook name> hook error` 通知后跟 stderr 的第一行。

For full execution details including which hooks matched, their exit codes, stdout, and stderr, read the debug log. Start Claude Code with `claude --debug-file /tmp/claude.log` to write to a known path, then `tail -f /tmp/claude.log` in another terminal. If you started without that flag, run `/debug` mid-session to enable logging and find the log path.

如需完整的执行详情（包括哪些 hook 匹配、退出码、stdout 和 stderr），请阅读 debug 日志。使用 `claude --debug-file /tmp/claude.log` 启动 Claude Code 以写入已知路径，然后在另一个终端中 `tail -f /tmp/claude.log`。如果你启动时未使用该标志，在会话中运行 `/debug` 以启用日志记录并找到日志路径。

### Learn more

- Hooks reference: full event schemas, JSON output format, async hooks, and MCP tool hooks
- Security considerations: review before deploying hooks in shared or production environments
- Bash command validator example: complete reference implementation

### 了解更多

- Hooks 参考文档：完整的事件 schema、JSON 输出格式、异步 hook 和 MCP 工具 hook
- 安全注意事项：在共享或生产环境中部署 hook 前请先审阅
- Bash 命令验证器示例：完整的参考实现

---TRANSLATION END---


---

## Claude Code - Common workflows（常用工作流）

原文链接：https://code.claude.com/docs/en/common-workflows

### Common workflows

> Step-by-step guides for exploring codebases, fixing bugs, refactoring, testing, and other everyday tasks with Claude Code.

> 分步指南：使用 Claude Code 探索代码库、修复缺陷、重构、测试以及完成其他日常任务。

This page collects short recipes for everyday development. For higher-level guidance on prompting and context management, see [Best practices](/en/best-practices).

本页面汇总了日常开发中的简短配方。如需关于提示词（prompt）与上下文管理的高阶指导，请参阅 [Best practices](/en/best-practices)。

This page covers:

本页内容涵盖：

* [Prompt recipes](#prompt-recipes) for exploring code, fixing bugs, refactoring, testing, PRs, and documentation
* [Resume previous conversations](#resume-previous-conversations) so a task can span multiple sittings
* [Run parallel sessions with worktrees](#run-parallel-sessions-with-worktrees) so concurrent edits don't collide
* [Plan before editing](#plan-before-editing) to review changes before they touch disk
* [Delegate research to subagents](#delegate-research-to-subagents) to keep your main context clean
* [Pipe Claude into scripts](#pipe-claude-into-scripts) for CI and batch processing

* [提示词配方](#prompt-recipes)：用于探索代码、修复缺陷、重构、测试、PR（pull request，合并请求）与文档
* [恢复之前的对话](#resume-previous-conversations)：让一项任务可以跨多次操作完成
* [用 worktree 并行运行会话](#run-parallel-sessions-with-worktrees)：避免并发编辑产生冲突
* [编辑前先规划](#plan-before-editing)：在改动落盘之前先审阅
* [将研究任务委派给 subagent](#delegate-research-to-subagents)：保持主上下文整洁
* [将 Claude 接入脚本](#pipe-claude-into-scripts)：用于 CI（持续集成）与批处理

### Prompt recipes

These are prompt patterns for everyday tasks like exploring unfamiliar code, debugging, refactoring, writing tests, and creating PRs. Each works in any Claude Code surface; adapt the wording to your project.

### 提示词配方

这些是针对探索陌生代码、调试、重构、编写测试、创建 PR 等日常任务的提示词模式。每一种都适用于任何 Claude Code 界面；请根据你的项目调整措辞。

### Understand new codebases

For configuring Claude Code in a monorepo or large codebase, see [Monorepos and large repos](/en/large-codebases).

### 理解新代码库

关于在 monorepo 或大型代码库中配置 Claude Code，请参阅 [Monorepos and large repos](/en/large-codebases)。

#### Get a quick codebase overview

Suppose you've just joined a new project and need to understand its structure quickly.

#### 快速了解代码库概览

假设你刚加入一个新项目，需要快速理解它的结构。

```bash theme={null}
cd /path/to/project
```

```bash theme={null}
claude
```

```text theme={null}
give me an overview of this codebase
```

（步骤：进入项目根目录；启动 Claude Code；请求一份代码库概览）

```text theme={null}
explain the main architecture patterns used here
```

```text theme={null}
what are the key data models?
```

```text theme={null}
how is authentication handled?
```

（随后深入特定组件：解释此处使用的主要架构模式、关键数据模型、认证是如何处理的）

Tip:

提示：

* Start with broad questions, then narrow down to specific areas
* Ask about coding conventions and patterns used in the project
* Request a glossary of project-specific terms

* 先问宽泛的问题，再缩小到具体领域
* 询问项目中使用的编码约定与模式
* 请 Claude 提供项目专属术语表

#### Find relevant code

Suppose you need to locate code related to a specific feature or functionality.

#### 查找相关代码

假设你需要定位与某个特性或功能相关的代码。

```text theme={null}
find the files that handle user authentication
```

```text theme={null}
how do these authentication files work together?
```

```text theme={null}
trace the login process from front-end to database
```

（步骤：请 Claude 查找相关文件；了解组件之间如何协作；追踪从前端到数据库的执行流程）

Tip:

提示：

* Be specific about what you're looking for
* Use domain language from the project
* Install a [code intelligence plugin](/en/discover-plugins#code-intelligence) for your language to give Claude precise "go to definition" and "find references" navigation

* 对你要找的内容描述得尽量具体
* 使用项目中的领域语言
* 为你的语言安装一个 [code intelligence plugin](/en/discover-plugins#code-intelligence)（代码智能插件），让 Claude 拥有精确的「跳转到定义」和「查找引用」导航能力

### Fix bugs efficiently

Suppose you've encountered an error message and need to find and fix its source.

### 高效修复缺陷

假设你遇到了一条错误信息，需要找到并修复其根源。

```text theme={null}
I'm seeing an error when I run npm test
```

```text theme={null}
suggest a few ways to fix the @ts-ignore in user.ts
```

```text theme={null}
update user.ts to add the null check you suggested
```

（步骤：把错误分享给 Claude；请求修复建议；应用修复）

Tip:

提示：

* Tell Claude the command to reproduce the issue and get a stack trace
* Mention any steps to reproduce the error
* Let Claude know if the error is intermittent or consistent

* 告诉 Claude 复现问题的命令并获取堆栈跟踪
* 说明复现错误的步骤
* 让 Claude 知道错误是偶发还是必现

### Refactor code

Suppose you need to update old code to use modern patterns and practices.

### 重构代码

假设你需要将旧代码更新为使用现代模式与实践。

```text theme={null}
find deprecated API usage in our codebase
```

```text theme={null}
suggest how to refactor utils.js to use modern JavaScript features
```

```text theme={null}
refactor utils.js to use ES2024 features while maintaining the same behavior
```

```text theme={null}
run tests for the refactored code
```

（步骤：识别待重构的遗留代码；获取重构建议；安全地应用改动；验证重构）

Tip:

提示：

* Ask Claude to explain the benefits of the modern approach
* Request that changes maintain backward compatibility when needed
* Do refactoring in small, testable increments

* 请 Claude 解释现代方案的好处
* 需要时要求改动保持向后兼容
* 以小而可测试的增量进行重构

### Work with tests

Suppose you need to add tests for uncovered code.

### 编写测试

假设你需要为未被覆盖的代码补充测试。

```text theme={null}
find functions in NotificationsService.swift that are not covered by tests
```

```text theme={null}
add tests for the notification service
```

```text theme={null}
add test cases for edge conditions in the notification service
```

```text theme={null}
run the new tests and fix any failures
```

（步骤：识别未测试的代码；生成测试骨架；添加有意义的测试用例；运行并验证测试）

Claude can generate tests that follow your project's existing patterns and conventions. When asking for tests, be specific about what behavior you want to verify. Claude examines your existing test files to match the style, frameworks, and assertion patterns already in use.

Claude 能够生成遵循你项目既有模式与约定的测试。在请求测试时，请明确说明你想验证的行为。Claude 会检查你现有的测试文件，以匹配已经在使用的风格、框架与断言模式。

For comprehensive coverage, ask Claude to identify edge cases you might have missed. Claude can analyze your code paths and suggest tests for error conditions, boundary values, and unexpected inputs that are easy to overlook.

为了获得全面的覆盖率，可以请 Claude 找出你可能遗漏的边界情况。Claude 能分析你的代码路径，并为错误条件、边界值以及容易被忽略的意外输入建议测试。

### Create pull requests

You can create pull requests by asking Claude directly ("create a pr for my changes"), or guide Claude through it step-by-step:

### 创建 pull request

你可以直接让 Claude 创建 PR（"create a pr for my changes"），也可以一步步引导 Claude 完成：

```text theme={null}
summarize the changes I've made to the authentication module
```

```text theme={null}
create a pr
```

```text theme={null}
enhance the PR description with more context about the security improvements
```

（步骤：总结你做的改动；生成 PR；审阅并完善）

When you create a PR using `gh pr create`, the session is automatically linked to that PR. To return to it later, run `claude --from-pr 123`, replacing 123 with the PR number, or paste the PR URL into the [`/resume` picker](/en/sessions#use-the-session-picker) search.

当你使用 `gh pr create` 创建 PR 时，会话会自动与该 PR 关联。之后想回到该会话，可运行 `claude --from-pr 123`（将 123 替换为 PR 编号），或在 [`/resume` 选择器](/en/sessions#use-the-session-picker)的搜索中粘贴 PR URL。

Review Claude's generated PR before submitting and ask Claude to highlight potential risks or considerations.

提交前请审阅 Claude 生成的 PR，并请 Claude 标出潜在风险或注意事项。

### Handle documentation

Suppose you need to add or update documentation for your code.

### 处理文档

假设你需要为代码添加或更新文档。

```text theme={null}
find functions without proper JSDoc comments in the auth module
```

```text theme={null}
add JSDoc comments to the undocumented functions in auth.js
```

```text theme={null}
improve the generated documentation with more context and examples
```

```text theme={null}
check if the documentation follows our project standards
```

（步骤：找出未文档化的代码；生成文档；审阅并增强；验证文档）

Tip:

提示：

* Specify the documentation style you want (JSDoc, docstrings, etc.)
* Ask for examples in the documentation
* Request documentation for public APIs, interfaces, and complex logic

* 指定你想要的文档风格（JSDoc、docstrings 等）
* 要求在文档中加入示例
* 为公开 API、接口和复杂逻辑请求文档

### Work in notes and non-code folders

Claude Code works in any directory. Run it inside a notes vault, a documentation folder, or any collection of markdown files to search, edit, and reorganize content the same way you would code.

### 在笔记与非代码文件夹中工作

Claude Code 可以在任何目录中工作。在笔记库、文档文件夹或任何 markdown 文件集合中运行它，即可像处理代码一样搜索、编辑和重组内容。

The `.claude/` directory and `CLAUDE.md` sit alongside other tools' config directories without conflict. Claude reads files fresh on each tool call, so it sees edits you make in another application the next time it reads that file.

`.claude/` 目录与 `CLAUDE.md` 与其他工具的配置目录并存，互不冲突。Claude 在每次工具调用时都会重新读取文件，因此你在其他应用中所做的编辑会在它下次读取该文件时被看到。

### Work with images

Suppose you need to work with images in your codebase, and you want Claude's help analyzing image content.

### 处理图片

假设你需要在代码库中处理图片，并希望 Claude 帮助分析图片内容。

1. Drag and drop an image into the Claude Code window
2. Copy an image and paste it into the CLI with ctrl+v (Do not use cmd+v)
3. Provide an image path to Claude. E.g., "Analyze this image: /path/to/your/image.png"

（添加图片到对话的三种方式：1. 将图片拖入 Claude Code 窗口；2. 复制图片后用 ctrl+v 粘贴到 CLI（不要用 cmd+v）；3. 给 Claude 一个图片路径，例如 "Analyze this image: /path/to/your/image.png"）

```text theme={null}
What does this image show?
```

```text theme={null}
Describe the UI elements in this screenshot
```

```text theme={null}
Are there any problematic elements in this diagram?
```

（请 Claude 分析图片：这张图片展示的是什么？描述截图中的 UI 元素；这张图中是否有问题元素）

```text theme={null}
Here's a screenshot of the error. What's causing it?
```

```text theme={null}
This is our current database schema. How should we modify it for the new feature?
```

（用图片提供上下文：这是错误的截图，是什么导致的？这是我们当前的数据库 schema，为新特性应如何修改？）

```text theme={null}
Generate CSS to match this design mockup
```

```text theme={null}
What HTML structure would recreate this component?
```

（从视觉内容获取代码建议：生成与该设计稿匹配的 CSS；什么样的 HTML 结构能复现这个组件？）

Tip:

提示：

* Use images when text descriptions would be unclear or cumbersome
* Include screenshots of errors, UI designs, or diagrams for better context
* You can work with multiple images in a conversation
* Image analysis works with diagrams, screenshots, mockups, and more
* When Claude references images (for example, `[Image #1]`), `Cmd+Click` (Mac) or `Ctrl+Click` (Windows/Linux) the link to open the image in your default viewer

* 当文字描述不清晰或繁琐时，使用图片
* 附上错误、UI 设计或示意图的截图以获得更好的上下文
* 你可以在一个对话中使用多张图片
* 图片分析适用于示意图、截图、设计稿等
* 当 Claude 引用图片（例如 `[Image #1]`）时，`Cmd+Click`（Mac）或 `Ctrl+Click`（Windows/Linux）该链接可在默认查看器中打开图片

### Reference files and directories

Use @ to quickly include files or directories without waiting for Claude to read them.

### 引用文件与目录

使用 @ 快速引入文件或目录，无需等待 Claude 去读取。

```text theme={null}
Explain the logic in @src/utils/auth.js
```

（引用单个文件：这会把该文件的完整内容引入对话）

```text theme={null}
What's the structure of @src/components?
```

（引用目录：这会提供带有文件信息的目录列表）

```text theme={null}
Show me the data from @github:repos/owner/repo/issues
```

（引用 MCP 资源：这会以 @server:resource 的格式从已连接的 MCP 服务器获取数据。详见 [MCP resources](/en/mcp#use-mcp-resources)）

Tip:

提示：

* File paths can be relative or absolute
* @ file references add `CLAUDE.md` in the file's directory and parent directories to context
* Directory references show file listings, not contents
* You can reference multiple files in a single message (for example, "@file1.js and @file2.js")

* 文件路径可以是相对路径或绝对路径
* @ 文件引用会把该文件所在目录及其父目录中的 `CLAUDE.md` 加入上下文
* 目录引用展示的是文件列表，而非内容
* 你可以在一条消息中引用多个文件（例如 "@file1.js and @file2.js"）

### Run Claude on a schedule

Suppose you want Claude to handle a task automatically on a recurring basis, like reviewing open PRs every morning, auditing dependencies weekly, or checking for CI failures overnight.

### 定时运行 Claude

假设你希望 Claude 自动周期性地处理某项任务，例如每天早上审阅开放的 PR、每周审计依赖、或在夜间检查 CI 失败。

Pick a scheduling option based on where you want the task to run:

根据你希望任务运行的位置选择一种调度方式：

| Option                                                 | Where it runs                     | Best for                                                                                                                                                                                                 |
| :----------------------------------------------------- | :-------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Routines](/en/routines)                               | Anthropic-managed infrastructure  | Tasks that should run even when your computer is off. Can also trigger on API calls or GitHub events in addition to a schedule. Configure at [claude.ai/code/routines](https://claude.ai/code/routines). |
| [Desktop scheduled tasks](/en/desktop-scheduled-tasks) | Your machine, via the desktop app | Tasks that need direct access to local files, tools, or uncommitted changes.                                                                                                                             |
| [GitHub Actions](/en/github-actions)                   | Your CI pipeline                  | Tasks tied to repo events like opened PRs, or cron schedules that should live alongside your workflow config.                                                                                            |
| [`/loop`](/en/scheduled-tasks)                         | The current CLI session           | Quick polling while a session is open. Tasks stop when you start a new conversation; `--resume` and `--continue` restore unexpired ones.                                                                 |

| 选项                                                    | 运行位置                          | 适用场景                                                                                                                                                                                                  |
| :------------------------------------------------------ | :-------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Routines](/en/routines)                                | Anthropic 托管的基础设施          | 即使电脑关机也应运行的任务。除定时外，还可由 API 调用或 GitHub 事件触发。在 [claude.ai/code/routines](https://claude.ai/code/routines) 配置。                                                              |
| [Desktop scheduled tasks](/en/desktop-scheduled-tasks)  | 你的本机，通过桌面应用            | 需要直接访问本地文件、工具或未提交改动的任务。                                                                                                                                                            |
| [GitHub Actions](/en/github-actions)                    | 你的 CI 流水线                    | 与仓库事件（如 PR 被打开）绑定的任务，或应与你的 workflow 配置共存的 cron 定时任务。                                                                                                                       |
| [`/loop`](/en/scheduled-tasks)                          | 当前 CLI 会话                     | 会话开启期间的快速轮询。开启新对话时任务停止；`--resume` 和 `--continue` 可恢复未过期的任务。                                                                                                              |

When writing prompts for scheduled tasks, be explicit about what success looks like and what to do with results. The task runs autonomously, so it can't ask clarifying questions. For example: "Review open PRs labeled `needs-review`, leave inline comments on any issues, and post a summary in the `#eng-reviews` Slack channel."

为定时任务编写提示词时，请明确说明成功标准是什么、结果如何处理。任务会自主运行，无法提出澄清性问题。例如：「审阅带 `needs-review` 标签的开放 PR，对任何问题留下行内评论，并在 `#eng-reviews` Slack 频道发布摘要。」

### Ask Claude about its capabilities

Claude has built-in access to its documentation and can answer questions about its own features and limitations.

### 向 Claude 询问它的能力

Claude 内置了对其文档的访问，能回答关于自身特性与限制的问题。

```text theme={null}
can Claude Code create pull requests?
```

```text theme={null}
how does Claude Code handle permissions?
```

```text theme={null}
what skills are available?
```

```text theme={null}
how do I use MCP with Claude Code?
```

```text theme={null}
how do I configure Claude Code for Amazon Bedrock?
```

```text theme={null}
what are the limitations of Claude Code?
```

（示例问题：Claude Code 能创建 PR 吗？Claude Code 如何处理权限？有哪些 skill 可用？如何配合 MCP 使用 Claude Code？如何为 Amazon Bedrock 配置 Claude Code？Claude Code 有哪些限制？）

Claude provides documentation-based answers to these questions. For hands-on demonstrations, run `/powerup` for interactive lessons with animated demos, or refer to the specific workflow sections above.

Claude 基于文档回答这些问题。如需上手演示，可运行 `/powerup` 获取带有动画演示的交互式课程，或参考上方具体的工作流章节。

Tip:

提示：

* Claude always has access to the latest Claude Code documentation, regardless of the version you're using
* Ask specific questions to get detailed answers
* Claude can explain complex features like MCP integration, enterprise configurations, and advanced workflows

* 无论你使用的是哪个版本，Claude 始终能访问最新的 Claude Code 文档
* 提出具体问题以获得详细回答
* Claude 能解释诸如 MCP 集成、企业配置和高级工作流等复杂特性

### Resume previous conversations

When a task spans multiple sittings, pick up where you left off instead of re-explaining context. Claude Code saves every conversation locally.

### 恢复之前的对话

当一项任务跨越多次操作时，从你上次中断处继续即可，不必重新解释上下文。Claude Code 会把每次对话保存在本地。

```bash theme={null}
claude --continue
```

这会恢复当前目录中最近的会话；如果尚不存在会话，则打印 `No conversation found to continue` 并退出。使用 `claude --resume` 可从列表中选择，或在运行中的会话内使用 `/resume`。关于命名、分支以及完整的选择器参考，请参阅 [Manage sessions](/en/sessions)。

### Run parallel sessions with worktrees

Work on a feature in one terminal while Claude fixes a bug in another, without the edits colliding. Each worktree is a separate checkout on its own branch.

### 用 worktree 并行运行会话

在一个终端开发特性的同时，让 Claude 在另一个终端修复缺陷，而两者的编辑互不冲突。每个 worktree 都是位于自己分支上的一个独立检出。

```bash theme={null}
claude --worktree feature-auth
```

在第二个终端用不同名称运行同一命令，即可启动一个隔离的并行会话。关于清理、`.worktreeinclude` 以及非 git 的 VCS 支持，请参阅 [Worktrees](/en/worktrees)。若想在一个屏幕而非多个终端中监控并行会话，请参阅 [background agents](/en/agent-view)。

### Plan before editing

For changes you want to review before they touch disk, switch to plan mode. Claude reads files and proposes a plan but makes no edits until you approve.

### 编辑前先规划

对于你想在落盘前审阅的改动，切换到 plan mode（规划模式）。Claude 会读取文件并提出计划，但在你批准前不做任何编辑。

```bash theme={null}
claude --permission-mode plan
```

你也可以在会话中按 `Shift+Tab` 切换到规划模式。关于审批流程以及在文本编辑器中编辑计划，请参阅 [Plan mode](/en/permission-modes#analyze-before-you-edit-with-plan-mode)。

### Delegate research to subagents

Exploring a large codebase fills your context with file reads. Delegate the exploration so only the findings come back.

### 将研究任务委派给 subagent

探索大型代码库会让你的上下文充斥着文件读取内容。把探索委派出去，只让结果返回即可。

```text theme={null}
use a subagent to investigate how our auth system handles token refresh
```

subagent（子代理）在自己的上下文窗口中读取文件并汇报摘要。关于用专属工具与提示词定义自定义 agent，请参阅 [Subagents](/en/sub-agents)。

### Pipe Claude into scripts

Run Claude non-interactively for CI, pre-commit hooks, or batch processing. Stdin and stdout work like any Unix tool.

### 将 Claude 接入脚本

以非交互方式运行 Claude，用于 CI、pre-commit hook（提交前钩子）或批处理。标准输入和标准输出的行为与任何 Unix 工具一致。

```bash theme={null}
git log --oneline -20 | claude -p "summarize these recent commits"
```

关于输出格式、权限标志和扇出模式，请参阅 [Non-interactive mode](/en/headless)。

### Next steps

### 下一步

* [Best practices](/en/best-practices) — Patterns for getting the most out of Claude Code
* [Manage sessions](/en/sessions) — Resume, name, and branch conversations
* [Worktrees](/en/worktrees) — Run isolated parallel sessions
* [Extend Claude Code](/en/features-overview) — Add skills, hooks, MCP, subagents, and plugins

* [Best practices](/en/best-practices) — 充分发挥 Claude Code 能力的模式
* [Manage sessions](/en/sessions) — 恢复、命名和分支对话
* [Worktrees](/en/worktrees) — 运行隔离的并行会话
* [Extend Claude Code](/en/features-overview) — 添加 skill（技能）、hook（钩子）、MCP、subagent 与 plugin（插件）


---

## Claude Code - How Claude remembers your project（Claude 如何记忆你的项目）

原文链接：https://code.claude.com/docs/en/memory

### How Claude remembers your project

> Give Claude persistent instructions with CLAUDE.md files, and let Claude accumulate learnings automatically with auto memory.

### Claude 如何记忆你的项目

> 使用 CLAUDE.md 文件向 Claude 提供持久化指令，并通过 auto memory（自动记忆）让 Claude 自动积累经验。

Each Claude Code session begins with a fresh context window. Two mechanisms carry knowledge across sessions:

* **CLAUDE.md files**: instructions you write to give Claude persistent context
* **Auto memory**: notes Claude writes itself based on your corrections and preferences

每个 Claude Code 会话都从一个全新的上下文窗口（context window）开始。有两种机制可以跨会话传递知识：

* **CLAUDE.md 文件**：你编写的指令，用于为 Claude 提供持久化上下文
* **Auto memory（自动记忆）**：Claude 根据你的纠正和偏好自己记录的笔记

This page covers how to:

* [Write and organize CLAUDE.md files](#claude-md-files)
* [Scope rules to specific file types](#organize-rules-with-claude/rules/) with `.claude/rules/`
* [Configure auto memory](#auto-memory) so Claude takes notes automatically
* [Troubleshoot](#troubleshoot-memory-issues) when instructions aren't being followed

本页涵盖以下内容：

* [编写和组织 CLAUDE.md 文件](#claude-md-files)
* [使用 `.claude/rules/` 将规则限定到特定文件类型](#organize-rules-with-claude/rules/)
* [配置 auto memory](#auto-memory)，让 Claude 自动做笔记
* 当指令未被遵循时进行[故障排查](#troubleshoot-memory-issues)

### CLAUDE.md vs auto memory

Claude Code has two complementary memory systems. Both are loaded at the start of every conversation. Claude treats them as context, not enforced configuration. To block an action regardless of what Claude decides, use a [PreToolUse hook](/en/hooks-guide) instead. The more specific and concise your instructions, the more consistently Claude follows them.

### CLAUDE.md 与 auto memory 的对比

Claude Code 拥有两套互补的记忆系统。两者都在每次对话开始时加载。Claude 将它们视为上下文，而非强制执行的配置。若要无论 Claude 如何决策都阻止某个操作，请改用 [PreToolUse hook（工具使用前钩子，即工具调用前触发的 shell 命令）](/en/hooks-guide)。指令越具体、越简洁，Claude 遵循的程度就越一致。

|                      | CLAUDE.md files                                   | Auto memory                                                      |
| :------------------- | :------------------------------------------------ | :--------------------------------------------------------------- |
| **Who writes it**    | You                                               | Claude                                                           |
| **What it contains** | Instructions and rules                            | Learnings and patterns                                           |
| **Scope**            | Project, user, or org                             | Per repository, shared across worktrees                          |
| **Loaded into**      | Every session                                     | Every session (first 200 lines or 25KB)                          |
| **Use for**          | Coding standards, workflows, project architecture | Build commands, debugging insights, preferences Claude discovers |

|                      | CLAUDE.md 文件                                    | Auto memory                                                      |
| :------------------- | :------------------------------------------------ | :--------------------------------------------------------------- |
| **由谁编写**         | 你                                                | Claude                                                           |
| **包含内容**         | 指令和规则                                        | 经验和模式                                                       |
| **作用范围**         | 项目、用户或组织                                  | 按仓库，跨 worktree 共享                                         |
| **加载位置**         | 每次会话                                          | 每次会话（前 200 行或 25KB）                                     |
| **适用场景**         | 编码规范、工作流、项目架构                        | 构建命令、调试经验、Claude 发现的偏好                            |

Use CLAUDE.md files when you want to guide Claude's behavior. Auto memory lets Claude learn from your corrections without manual effort.

Subagents can also maintain their own auto memory. See [subagent configuration](/en/sub-agents#enable-persistent-memory) for details.

当你希望引导 Claude 的行为时，请使用 CLAUDE.md 文件。auto memory 则让 Claude 无需手动操作即可从你的纠正中学习。

subagent（子智能体，即主智能体委派任务的从属智能体）也可以维护各自的 auto memory。详情请参阅 [subagent 配置](/en/sub-agents#enable-persistent-memory)。

### CLAUDE.md files

CLAUDE.md files are markdown files that give Claude persistent instructions for a project, your personal workflow, or your entire organization. You write these files in plain text; Claude reads them at the start of every session.

### CLAUDE.md 文件

CLAUDE.md 文件是 markdown 文件，用于为项目、你的个人工作流或整个组织提供持久化指令。你以纯文本形式编写这些文件；Claude 在每次会话开始时读取它们。

### When to add to CLAUDE.md

Treat CLAUDE.md as the place you write down what you'd otherwise re-explain. Add to it when:

* Claude makes the same mistake a second time
* A code review catches something Claude should have known about this codebase
* You type the same correction or clarification into chat that you typed last session
* A new teammate would need the same context to be productive

### 何时向 CLAUDE.md 添加内容

把 CLAUDE.md 当作你写下"否则又要重新解释一遍"的内容的地方。在以下情况时添加：

* Claude 第二次犯同样的错误
* code review（代码审查）发现了 Claude 本应了解的关于此代码库的内容
* 你在聊天中输入了和上次会话相同的纠正或澄清
* 一位新队友需要相同的上下文才能高效工作

Keep it to facts Claude should hold in every session: build commands, conventions, project layout, "always do X" rules. If an entry is a multi-step procedure or only matters for one part of the codebase, move it to a [skill](/en/skills) or a [path-scoped rule](#organize-rules-with-claude/rules/) instead. The [extension overview](/en/features-overview#build-your-setup-over-time) covers when to use each mechanism.

只保留 Claude 在每次会话中都应掌握的事实：构建命令、约定、项目布局、"始终做 X"的规则。如果某条内容是多步骤流程，或只与代码库的某一部分相关，请将其移至 [skill（技能，即可按需打包加载的重复性工作流）](/en/skills)或[路径限定规则](#organize-rules-with-claude/rules/)。[扩展概览](/en/features-overview#build-your-setup-over-time)介绍了何时使用每种机制。

### Choose where to put CLAUDE.md files

CLAUDE.md files can live in several locations, each with a different scope. The table below lists them in load order, from broadest scope to most specific, so a project instruction appears in context after a user instruction.

### 选择 CLAUDE.md 文件的放置位置

CLAUDE.md 文件可以放在多个位置，每个位置对应不同的作用范围。下表按加载顺序列出，从最广到最窄的作用范围，因此项目指令会在用户指令之后出现在上下文中。

| Scope                    | Location                                                                                                                                                                | Purpose                                                    | Use case examples                                                    | Shared with                     |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------- | ------------------------------- |
| **Managed policy**       | • macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`<br />• Linux and WSL: `/etc/claude-code/CLAUDE.md`<br />• Windows: `C:\Program Files\ClaudeCode\CLAUDE.md` | Organization-wide instructions managed by IT/DevOps        | Company coding standards, security policies, compliance requirements | All users in organization       |
| **User instructions**    | `~/.claude/CLAUDE.md`                                                                                                                                                   | Personal preferences for all projects                      | Code styling preferences, personal tooling shortcuts                 | Just you (all projects)         |
| **Project instructions** | `./CLAUDE.md` or `./.claude/CLAUDE.md`                                                                                                                                  | Team-shared instructions for the project                   | Project architecture, coding standards, common workflows             | Team members via source control |
| **Local instructions**   | `./CLAUDE.local.md`                                                                                                                                                     | Personal project-specific preferences; add to `.gitignore` | Your sandbox URLs, preferred test data                               | Just you (current project)      |

| 作用范围                 | 位置                                                                                                                                                                    | 用途                                       | 使用示例                                       | 共享对象                         |
| ------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ | ---------------------------------------------- | -------------------------------- |
| **托管策略**             | • macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`<br />• Linux 和 WSL: `/etc/claude-code/CLAUDE.md`<br />• Windows: `C:\Program Files\ClaudeCode\CLAUDE.md` | 由 IT/DevOps 管理的组织级指令              | 公司编码规范、安全策略、合规要求               | 组织内所有用户                   |
| **用户指令**             | `~/.claude/CLAUDE.md`                                                                                                                                                   | 适用于所有项目的个人偏好                   | 代码风格偏好、个人工具快捷方式                 | 仅你自己（所有项目）             |
| **项目指令**             | `./CLAUDE.md` 或 `./.claude/CLAUDE.md`                                                                                                                                  | 项目级团队共享指令                         | 项目架构、编码规范、常用工作流                 | 通过版本控制共享给团队成员       |
| **本地指令**             | `./CLAUDE.local.md`                                                                                                                                                     | 项目级的个人偏好；应加入 `.gitignore`      | 你的 sandbox（沙箱）URL、首选测试数据          | 仅你自己（当前项目）             |

CLAUDE.md and CLAUDE.local.md files in the directory hierarchy above the working directory are loaded in full at launch. Files in subdirectories load on demand when Claude reads files in those directories. See [How CLAUDE.md files load](#how-claude-md-files-load) for the full resolution order.

工作目录上层目录结构中的 CLAUDE.md 和 CLAUDE.local.md 文件会在启动时完整加载。子目录中的文件则在 Claude 读取这些子目录中的文件时按需加载。完整的解析顺序请参阅 [CLAUDE.md 文件如何加载](#how-claude-md-files-load)。

For large projects, you can break instructions into topic-specific files using [project rules](#organize-rules-with-claude/rules/). Rules let you scope instructions to specific file types or subdirectories.

对于大型项目，你可以使用[项目规则](#organize-rules-with-claude/rules/)将指令拆分为按主题划分的文件。规则允许你将指令限定到特定文件类型或子目录。

### Set up a project CLAUDE.md

A project CLAUDE.md can be stored in either `./CLAUDE.md` or `./.claude/CLAUDE.md`. Create this file and add instructions that apply to anyone working on the project: build and test commands, coding standards, architectural decisions, naming conventions, and common workflows. These instructions are shared with your team through version control, so focus on project-level standards rather than personal preferences.

### 设置项目 CLAUDE.md

项目 CLAUDE.md 可以存放在 `./CLAUDE.md` 或 `./.claude/CLAUDE.md`。创建此文件并添加适用于任何参与该项目的人的指令：构建和测试命令、编码规范、架构决策、命名约定以及常用工作流。这些指令通过版本控制与团队共享，因此应聚焦于项目级标准而非个人偏好。

Run `/init` to generate a starting CLAUDE.md automatically. Claude analyzes your codebase and creates a file with build commands, test instructions, and project conventions it discovers. If a CLAUDE.md already exists, `/init` suggests improvements rather than overwriting it. Refine from there with instructions Claude wouldn't discover on its own.

运行 `/init` 可自动生成一个初始 CLAUDE.md。Claude 会分析你的代码库，并创建一个包含它所发现的构建命令、测试指令和项目约定的文件。如果 CLAUDE.md 已存在，`/init` 会建议改进而非覆盖它。在此基础上补充 Claude 无法自行发现的指令。

Set `CLAUDE_CODE_NEW_INIT=1` to enable an interactive multi-phase flow. `/init` asks which artifacts to set up: CLAUDE.md files, skills, and hooks. It then explores your codebase with a subagent, fills in gaps via follow-up questions, and presents a reviewable proposal before writing any files.

设置 `CLAUDE_CODE_NEW_INIT=1` 可启用交互式多阶段流程。`/init` 会询问要设置哪些产物：CLAUDE.md 文件、skills 和 hooks（钩子，即生命周期事件触发的 shell 命令）。随后它会用 subagent 探索你的代码库，通过追问填补信息缺口，并在写入任何文件前呈现一份可审阅的方案。

### Write effective instructions

CLAUDE.md files are loaded into the context window at the start of every session, consuming tokens alongside your conversation. The [context window visualization](/en/context-window) shows where CLAUDE.md loads relative to the rest of the startup context. Because they're context rather than enforced configuration, how you write instructions affects how reliably Claude follows them. Specific, concise, well-structured instructions work best.

### 编写有效的指令

CLAUDE.md 文件在每次会话开始时加载到上下文窗口，与你的对话一起消耗 token。[上下文窗口可视化](/en/context-window)展示了 CLAUDE.md 相对于其余启动上下文的加载位置。由于它们是上下文而非强制执行的配置，你编写指令的方式会影响 Claude 遵循它们的可靠程度。具体、简洁、结构良好的指令效果最佳。

**Size**: target under 200 lines per CLAUDE.md file. Longer files consume more context and reduce adherence. If your instructions are growing large, use [path-scoped rules](#path-specific-rules) so instructions load only when Claude works with matching files. You can also split content into [imports](#import-additional-files) for organization, though imported files still load and enter the context window at launch.

**Structure**: use markdown headers and bullets to group related instructions. Claude scans structure the same way readers do: organized sections are easier to follow than dense paragraphs.

**Size（篇幅）**：每个 CLAUDE.md 文件控制在 200 行以内。过长的文件会消耗更多上下文并降低遵循度。如果你的指令越来越多，请使用[路径限定规则](#path-specific-rules)，这样指令只会在 Claude 处理匹配文件时才加载。你也可以将内容拆分为[导入](#import-additional-files)以便组织，但导入的文件仍会在启动时加载并进入上下文窗口。

**Structure（结构）**：使用 markdown 标题和列表将相关指令分组。Claude 扫描结构的方式与读者相同：有组织的段落比密集的段落更易于遵循。

**Specificity**: write instructions that are concrete enough to verify. For example:

* "Use 2-space indentation" instead of "Format code properly"
* "Run `npm test` before committing" instead of "Test your changes"
* "API handlers live in `src/api/handlers/`" instead of "Keep files organized"

**Specificity（具体性）**：编写足够具体、可验证的指令。例如：

* 用"使用 2 空格缩进"而非"正确格式化代码"
* 用"提交前运行 `npm test`"而非"测试你的改动"
* 用"API 处理器位于 `src/api/handlers/`"而非"保持文件有序"

**Consistency**: if two rules contradict each other, Claude may pick one arbitrarily. Review your CLAUDE.md files, nested CLAUDE.md files in subdirectories, and [`.claude/rules/`](#organize-rules-with-claude/rules/) periodically to remove outdated or conflicting instructions. In monorepos, use [`claudeMdExcludes`](#exclude-specific-claude-md-files) to skip CLAUDE.md files from other teams that aren't relevant to your work.

**Consistency（一致性）**：如果两条规则相互矛盾，Claude 可能任意选择其中一条。定期检查你的 CLAUDE.md 文件、子目录中的嵌套 CLAUDE.md 文件以及 [`.claude/rules/`](#organize-rules-with-claude/rules/)，移除过时或冲突的指令。在 monorepo 中，使用 [`claudeMdExcludes`](#exclude-specific-claude-md-files) 跳过其他团队与你工作无关的 CLAUDE.md 文件。

### Import additional files

CLAUDE.md files can import additional files using `@path/to/import` syntax. Imported files are expanded and loaded into context at launch alongside the CLAUDE.md that references them.

### 导入额外文件

CLAUDE.md 文件可以使用 `@path/to/import` 语法导入额外文件。导入的文件会在启动时展开并连同引用它的 CLAUDE.md 一起加载到上下文。

Both relative and absolute paths are allowed. Relative paths resolve relative to the file containing the import, not the working directory. Imported files can recursively import other files, with a maximum depth of four hops.

相对路径和绝对路径均可使用。相对路径相对于包含导入语句的文件解析，而非工作目录。导入的文件可以递归导入其他文件，最大深度为四层。

Import parsing skips Markdown code spans and fenced code blocks. To mention a path in your CLAUDE.md without importing it, wrap it in backticks: writing `` `@README` `` keeps the text literal, while `@README` outside backticks imports the file.

导入解析会跳过 Markdown 代码跨度和围栏代码块。若要在 CLAUDE.md 中提及某个路径而不导入它，请用反引号包裹：写 `` `@README` `` 会保留字面文本，而在反引号之外的 `@README` 则会导入该文件。

To pull in a README, package.json, and a workflow guide, reference them with `@` syntax anywhere in your CLAUDE.md:

```text theme={null}
See @README for project overview and @package.json for available npm commands for this project.

# Additional Instructions
- git workflow @docs/git-instructions.md
```

要在 CLAUDE.md 中引入 README、package.json 和工作流指南，可使用 `@` 语法在任意位置引用它们：

```text theme={null}
See @README for project overview and @package.json for available npm commands for this project.

# Additional Instructions
- git workflow @docs/git-instructions.md
```

For private per-project preferences that shouldn't be checked into version control, create a `CLAUDE.local.md` at the project root. It loads alongside `CLAUDE.md` and is treated the same way. Add `CLAUDE.local.md` to your `.gitignore` so it isn't committed; running `/init` and choosing the personal option does this for you.

对于不应提交到版本控制的私有项目偏好，请在项目根目录创建 `CLAUDE.local.md`。它与 `CLAUDE.md` 一起加载，处理方式相同。将 `CLAUDE.local.md` 加入 `.gitignore` 以免被提交；运行 `/init` 并选择个人选项即可自动完成。

If you work across multiple git worktrees of the same repository, a gitignored `CLAUDE.local.md` only exists in the worktree where you created it. To share personal instructions across worktrees, import a file from your home directory instead:

```text theme={null}
# Individual Preferences
- @~/.claude/my-project-instructions.md
```

如果你在同一仓库的多个 git worktree 中工作，被 gitignore 的 `CLAUDE.local.md` 只存在于创建它的那个 worktree 中。若要跨 worktree 共享个人指令，请改为从家目录导入文件：

```text theme={null}
# Individual Preferences
- @~/.claude/my-project-instructions.md
```

The first time Claude Code encounters external imports in a project, it shows an approval dialog listing the files. If you decline, the imports stay disabled and the dialog does not appear again.

当 Claude Code 首次在项目中遇到外部导入时，会显示一个列出这些文件的批准对话框。如果你拒绝，这些导入将保持禁用状态，且该对话框不再出现。

For a more structured approach to organizing instructions, see [`.claude/rules/`](#organize-rules-with-claude/rules/).

如需更结构化的指令组织方式，请参阅 [`.claude/rules/`](#organize-rules-with-claude/rules/)。

### AGENTS.md

Claude Code reads `CLAUDE.md`, not `AGENTS.md`. If your repository already uses `AGENTS.md` for other coding agents, create a `CLAUDE.md` that imports it so both tools read the same instructions without duplicating them. You can also add Claude-specific instructions below the import. Claude loads the imported file at session start, then appends the rest:

### AGENTS.md

Claude Code 读取的是 `CLAUDE.md`，而非 `AGENTS.md`。如果你的仓库已在使用 `AGENTS.md` 供其他编码 agent（智能体）使用，请创建一个导入它的 `CLAUDE.md`，这样两个工具读取相同指令而无需重复。你也可以在导入语句下方添加 Claude 专属指令。Claude 会在会话开始时加载导入的文件，然后追加其余内容：

```markdown CLAUDE.md theme={null}
@AGENTS.md

## Claude Code

Use plan mode for changes under `src/billing/`.
```

```markdown CLAUDE.md theme={null}
@AGENTS.md

## Claude Code

Use plan mode for changes under `src/billing/`.
```

A symlink also works if you don't need to add Claude-specific content:

```bash theme={null}
ln -s AGENTS.md CLAUDE.md
```

如果你不需要添加 Claude 专属内容，也可以使用软链接：

```bash theme={null}
ln -s AGENTS.md CLAUDE.md
```

On Windows, creating a symlink requires Administrator privileges or Developer Mode, so use the `@AGENTS.md` import instead.

在 Windows 上，创建软链接需要管理员权限或开发者模式，因此请改用 `@AGENTS.md` 导入。

Running [`/init`](/en/commands) in a repo that already has an `AGENTS.md` reads it and incorporates the relevant parts into the generated `CLAUDE.md`. It also reads other tool configs like `.cursorrules`, `.devin/rules/`, and `.windsurfrules`.

在一个已有 `AGENTS.md` 的仓库中运行 [`/init`](/en/commands)，会读取它并将相关部分整合进生成的 `CLAUDE.md`。它还会读取其他工具配置，如 `.cursorrules`、`.devin/rules/` 和 `.windsurfrules`。

### How CLAUDE.md files load

Claude Code reads CLAUDE.md files by walking up the directory tree from your current working directory, checking each directory along the way for `CLAUDE.md` and `CLAUDE.local.md` files. This means if you run Claude Code in `foo/bar/`, it loads instructions from `foo/bar/CLAUDE.md`, `foo/CLAUDE.md`, and any `CLAUDE.local.md` files alongside them.

### CLAUDE.md 文件如何加载

Claude Code 从你的当前工作目录开始沿目录树向上遍历，逐个检查沿途每个目录中是否存在 `CLAUDE.md` 和 `CLAUDE.local.md` 文件。这意味着如果你在 `foo/bar/` 中运行 Claude Code，它会加载来自 `foo/bar/CLAUDE.md`、`foo/CLAUDE.md` 以及它们旁边的任何 `CLAUDE.local.md` 文件的指令。

All discovered files are concatenated into context rather than overriding each other. Across the directory tree, content is ordered from the filesystem root down to your working directory. For the `foo/bar/` example, `foo/CLAUDE.md` appears in context before `foo/bar/CLAUDE.md`, so instructions closer to where you launched Claude are read last. Within each directory, `CLAUDE.local.md` is appended after `CLAUDE.md`, so your personal notes are the last thing Claude reads at that level.

所有发现的文件会被拼接到上下文中，而非相互覆盖。在整个目录树中，内容按从文件系统根目录到工作目录的顺序排列。对于 `foo/bar/` 的例子，`foo/CLAUDE.md` 在上下文中出现在 `foo/bar/CLAUDE.md` 之前，因此离你启动 Claude 位置越近的指令越晚被读取。在每个目录内，`CLAUDE.local.md` 追加在 `CLAUDE.md` 之后，因此你的个人笔记是 Claude 在该层级最后读取的内容。

Claude also discovers `CLAUDE.md` and `CLAUDE.local.md` files in subdirectories under your current working directory. Instead of loading them at launch, they are included when Claude reads files in those subdirectories.

Claude 还会发现当前工作目录下子目录中的 `CLAUDE.md` 和 `CLAUDE.local.md` 文件。它们不会在启动时加载，而是在 Claude 读取这些子目录中的文件时才被纳入。

If you work in a large monorepo where other teams' CLAUDE.md files get picked up, use [`claudeMdExcludes`](#exclude-specific-claude-md-files) to skip them. For the full layout of root and per-directory CLAUDE.md files and rules, see [Monorepos and large repos](/en/large-codebases).

如果你在大型 monorepo 中工作，其他团队的 CLAUDE.md 文件可能被加载，请使用 [`claudeMdExcludes`](#exclude-specific-claude-md-files) 跳过它们。关于根目录和各目录的 CLAUDE.md 文件及规则的完整布局，请参阅 [Monorepos 与大型仓库](/en/large-codebases)。

Block-level HTML comments (`<!-- maintainer notes -->`) in CLAUDE.md files are stripped before the content is injected into Claude's context. Use them to leave notes for human maintainers without spending context tokens on them. Comments inside code blocks are preserved. When you open a CLAUDE.md file directly with the Read tool, comments remain visible.

CLAUDE.md 文件中的块级 HTML 注释（`<!-- maintainer notes -->`）在内容注入 Claude 上下文之前会被剥离。你可以用它们为人类维护者留下备注，而无需消耗上下文 token。代码块内的注释会被保留。当你用 Read 工具直接打开 CLAUDE.md 文件时，注释仍然可见。

#### Load from additional directories

The `--add-dir` flag gives Claude access to additional directories outside your main working directory. By default, CLAUDE.md files from these directories are not loaded.

#### 从额外目录加载

`--add-dir` 标志让 Claude 可以访问主工作目录之外的额外目录。默认情况下，这些目录中的 CLAUDE.md 文件不会被加载。

To also load memory files from additional directories, set the `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` environment variable:

```bash theme={null}
CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1 claude --add-dir ../shared-config
```

若也要从额外目录加载记忆文件，请设置 `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD` 环境变量：

```bash theme={null}
CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1 claude --add-dir ../shared-config
```

This loads `CLAUDE.md`, `.claude/CLAUDE.md`, `.claude/rules/*.md`, and `CLAUDE.local.md` from the additional directory. `CLAUDE.local.md` is skipped if you exclude `local` from [`--setting-sources`](/en/cli-reference).

这会从额外目录加载 `CLAUDE.md`、`.claude/CLAUDE.md`、`.claude/rules/*.md` 和 `CLAUDE.local.md`。如果你在 [`--setting-sources`](/en/cli-reference) 中排除了 `local`，则跳过 `CLAUDE.local.md`。

### Organize rules with `.claude/rules/`

For larger projects, you can organize instructions into multiple files using the `.claude/rules/` directory. This keeps instructions modular and easier for teams to maintain. Rules can also be [scoped to specific file paths](#path-specific-rules), so they only load into context when Claude works with matching files, reducing noise and saving context space.

### 使用 `.claude/rules/` 组织规则

对于较大的项目，你可以使用 `.claude/rules/` 目录将指令组织为多个文件。这能让指令保持模块化，便于团队维护。规则还可以[限定到特定文件路径](#path-specific-rules)，这样只会在 Claude 处理匹配文件时才加载到上下文，从而减少噪声并节省上下文空间。

Rules load into context every session or when matching files are opened. For task-specific instructions that don't need to be in context all the time, use [skills](/en/skills) instead, which only load when you invoke them or when Claude determines they're relevant to your prompt.

规则会在每次会话或打开匹配文件时加载到上下文。对于不需要始终处于上下文中的任务专属指令，请改用 [skills](/en/skills)，它们只在你调用或 Claude 判定与你的 prompt（提示词）相关时才加载。

#### Set up rules

Place markdown files in your project's `.claude/rules/` directory. Each file should cover one topic, with a descriptive filename like `testing.md` or `api-design.md`. All `.md` files are discovered recursively, so you can organize rules into subdirectories like `frontend/` or `backend/`:

#### 设置规则

将 markdown 文件放在项目的 `.claude/rules/` 目录中。每个文件应覆盖一个主题，使用描述性文件名如 `testing.md` 或 `api-design.md`。所有 `.md` 文件会被递归发现，因此你可以将规则组织到子目录中，如 `frontend/` 或 `backend/`：

```text theme={null}
your-project/
├── .claude/
│   ├── CLAUDE.md           # Main project instructions
│   └── rules/
│       ├── code-style.md   # Code style guidelines
│       ├── testing.md      # Testing conventions
│       └── security.md     # Security requirements
```

```text theme={null}
your-project/
├── .claude/
│   ├── CLAUDE.md           # Main project instructions
│   └── rules/
│       ├── code-style.md   # Code style guidelines
│       ├── testing.md      # Testing conventions
│       └── security.md     # Security requirements
```

Rules without [`paths` frontmatter](#path-specific-rules) are loaded at launch with the same priority as `.claude/CLAUDE.md`.

没有 [`paths` frontmatter](#path-specific-rules) 的规则会在启动时加载，优先级与 `.claude/CLAUDE.md` 相同。

#### Path-specific rules

Rules can be scoped to specific files using YAML frontmatter with the `paths` field. These conditional rules only apply when Claude is working with files matching the specified patterns.

#### 路径限定规则

规则可以通过带 `paths` 字段的 YAML frontmatter 限定到特定文件。这些条件规则只在 Claude 处理匹配指定模式的文件时才生效。

```markdown theme={null}
---
paths:
  - "src/api/**/*.ts"
---

# API Development Rules

- All API endpoints must include input validation
- Use the standard error response format
- Include OpenAPI documentation comments
```

```markdown theme={null}
---
paths:
  - "src/api/**/*.ts"
---

# API Development Rules

- All API endpoints must include input validation
- Use the standard error response format
- Include OpenAPI documentation comments
```

Rules without a `paths` field are loaded unconditionally and apply to all files. Path-scoped rules trigger when Claude reads files matching the pattern, not on every tool use. As of v2.1.198, matching also works when Claude reaches a file through a symlinked path to the project directory, for example in a symlinked checkout.

没有 `paths` 字段的规则会无条件加载并应用于所有文件。路径限定规则在 Claude 读取匹配模式的文件时触发，而非每次工具调用都触发。自 v2.1.198 起，当 Claude 通过指向项目目录的软链接路径访问文件时（例如在软链接检出中），匹配同样生效。

Use glob patterns in the `paths` field to match files by extension, directory, or any combination:

在 `paths` 字段中使用 glob 模式，可按扩展名、目录或任意组合匹配文件：

| Pattern                | Matches                                  |
| ---------------------- | ---------------------------------------- |
| `**/*.ts`              | All TypeScript files in any directory    |
| `src/**/*`             | All files under `src/` directory         |
| `*.md`                 | Markdown files in the project root       |
| `src/components/*.tsx` | React components in a specific directory |

| 模式                   | 匹配                                      |
| ---------------------- | ----------------------------------------- |
| `**/*.ts`              | 任意目录下的所有 TypeScript 文件          |
| `src/**/*`             | `src/` 目录下的所有文件                   |
| `*.md`                 | 项目根目录下的 markdown 文件              |
| `src/components/*.tsx` | 特定目录下的 React 组件                   |

You can specify multiple patterns and use brace expansion to match multiple extensions in one pattern:

你可以指定多个模式，并使用花括号扩展在一个模式中匹配多个扩展名：

```markdown theme={null}
---
paths:
  - "src/**/*.{ts,tsx}"
  - "lib/**/*.ts"
  - "tests/**/*.test.ts"
---
```

```markdown theme={null}
---
paths:
  - "src/**/*.{ts,tsx}"
  - "lib/**/*.ts"
  - "tests/**/*.test.ts"
---
```

#### Share rules across projects with symlinks

The `.claude/rules/` directory supports symlinks, so you can maintain a shared set of rules and link them into multiple projects. Symlinks are resolved and loaded normally, and circular symlinks are detected and handled gracefully.

#### 通过软链接跨项目共享规则

`.claude/rules/` 目录支持软链接，因此你可以维护一组共享规则并将其链接到多个项目中。软链接会被正常解析和加载，循环软链接也会被检测并优雅处理。

This example links both a shared directory and an individual file:

```bash theme={null}
ln -s ~/shared-claude-rules .claude/rules/shared
ln -s ~/company-standards/security.md .claude/rules/security.md
```

此示例同时链接了一个共享目录和一个单独文件：

```bash theme={null}
ln -s ~/shared-claude-rules .claude/rules/shared
ln -s ~/company-standards/security.md .claude/rules/security.md
```

#### User-level rules

Personal rules in `~/.claude/rules/` apply to every project on your machine. Use them for preferences that aren't project-specific:

```text theme={null}
~/.claude/rules/
├── preferences.md    # Your personal coding preferences
└── workflows.md      # Your preferred workflows
```

#### 用户级规则

`~/.claude/rules/` 中的个人规则适用于你机器上的每个项目。用于与项目无关的偏好：

```text theme={null}
~/.claude/rules/
├── preferences.md    # Your personal coding preferences
└── workflows.md      # Your preferred workflows
```

User-level rules are loaded before project rules, giving project rules higher priority.

用户级规则在项目规则之前加载，因此项目规则具有更高优先级。

### Manage CLAUDE.md for large teams

For organizations deploying Claude Code across teams, you can centralize instructions and control which CLAUDE.md files are loaded.

### 为大型团队管理 CLAUDE.md

对于在团队间部署 Claude Code 的组织，你可以集中管理指令并控制加载哪些 CLAUDE.md 文件。

#### Deploy organization-wide CLAUDE.md

Organizations can deploy a centrally managed CLAUDE.md that applies to all users on a machine. This file cannot be excluded by individual settings.

#### 部署组织级 CLAUDE.md

组织可以部署一个集中管理的 CLAUDE.md，适用于某台机器上的所有用户。此文件无法被个人设置排除。

* macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`
* Linux and WSL: `/etc/claude-code/CLAUDE.md`
* Windows: `C:\Program Files\ClaudeCode\CLAUDE.md`

* macOS: `/Library/Application Support/ClaudeCode/CLAUDE.md`
* Linux 和 WSL: `/etc/claude-code/CLAUDE.md`
* Windows: `C:\Program Files\ClaudeCode\CLAUDE.md`

Use MDM, Group Policy, Ansible, or similar tools to distribute the file across developer machines. See [managed settings](/en/permissions#managed-settings) for other organization-wide configuration options.

使用 MDM、组策略、Ansible 或类似工具将文件分发到各开发者机器。其他组织级配置选项请参阅[托管设置](/en/permissions#managed-settings)。

The `claudeMd` key lets you put managed CLAUDE.md content directly inside `managed-settings.json` instead of deploying a separate file.

`claudeMd` 键允许你将托管的 CLAUDE.md 内容直接放入 `managed-settings.json`，而无需部署单独的文件。

**Scope**: every Claude Code session on the machine, in every repository. For repository-specific guidance, commit a project CLAUDE.md instead.

**Precedence**: same as a managed CLAUDE.md file. Loads before user and project CLAUDE.md.

**作用范围**：该机器上的每个 Claude Code 会话，覆盖每个仓库。若需仓库专属指引，请改为提交项目 CLAUDE.md。

**优先级**：与托管 CLAUDE.md 文件相同。在用户和项目 CLAUDE.md 之前加载。

**Where it's honored**: managed and policy settings only. Setting `claudeMd` in user, project, or local settings has no effect.

**生效位置**：仅在托管和策略设置中有效。在用户、项目或本地设置中设置 `claudeMd` 无效。

The example below adds behavioral instructions directly in a managed settings file:

```json theme={null}
{
  "claudeMd": "Always run `make lint` before committing.\nNever push directly to main."
}
```

以下示例直接在托管设置文件中添加行为指令：

```json theme={null}
{
  "claudeMd": "Always run `make lint` before committing.\nNever push directly to main."
}
```

A managed CLAUDE.md and [managed settings](/en/settings#settings-files) serve different purposes. Use settings for technical enforcement and CLAUDE.md for behavioral guidance:

托管 CLAUDE.md 与[托管设置](/en/settings#settings-files)用途不同。设置用于技术强制执行，CLAUDE.md 用于行为指引：

| Concern                                        | Configure in                                              |
| :--------------------------------------------- | :-------------------------------------------------------- |
| Block specific tools, commands, or file paths  | Managed settings: `permissions.deny`                      |
| Enforce sandbox isolation                      | Managed settings: `sandbox.enabled`                       |
| Environment variables and API provider routing | Managed settings: `env`                                   |
| Authentication method and organization lock    | Managed settings: `forceLoginMethod`, `forceLoginOrgUUID` |
| Code style and quality guidelines              | Managed CLAUDE.md                                         |
| Data handling and compliance reminders         | Managed CLAUDE.md                                         |
| Behavioral instructions for Claude             | Managed CLAUDE.md                                         |

| 关注点                                         | 配置位置                                                  |
| :--------------------------------------------- | :-------------------------------------------------------- |
| 阻止特定工具、命令或文件路径                   | 托管设置：`permissions.deny`                              |
| 强制 sandbox 隔离                              | 托管设置：`sandbox.enabled`                               |
| 环境变量和 API 提供商路由                      | 托管设置：`env`                                           |
| 认证方式和组织锁定                             | 托管设置：`forceLoginMethod`、`forceLoginOrgUUID`         |
| 代码风格和质量准则                             | 托管 CLAUDE.md                                            |
| 数据处理和合规提醒                             | 托管 CLAUDE.md                                            |
| 针对 Claude 的行为指令                         | 托管 CLAUDE.md                                            |

Settings rules are enforced by the client regardless of what Claude decides to do. CLAUDE.md instructions shape Claude's behavior but are not a hard enforcement layer.

设置规则由客户端强制执行，无论 Claude 如何决策。CLAUDE.md 指令塑造 Claude 的行为，但并非硬性强制层。

#### Exclude specific CLAUDE.md files

In large monorepos, ancestor CLAUDE.md files may contain instructions that aren't relevant to your work. The `claudeMdExcludes` setting lets you skip specific files by path or glob pattern.

#### 排除特定 CLAUDE.md 文件

在大型 monorepo 中，祖先目录的 CLAUDE.md 文件可能包含与你工作无关的指令。`claudeMdExcludes` 设置允许你按路径或 glob 模式跳过特定文件。

This example excludes a top-level CLAUDE.md and a rules directory from a parent folder. Add it to `.claude/settings.local.json` so the exclusion stays local to your machine:

```json theme={null}
{
  "claudeMdExcludes": [
    "**/monorepo/CLAUDE.md",
    "/home/user/monorepo/other-team/.claude/rules/**"
  ]
}
```

此示例排除了一个顶层 CLAUDE.md 和父文件夹中的一个 rules 目录。将其添加到 `.claude/settings.local.json`，使排除仅在你本机生效：

```json theme={null}
{
  "claudeMdExcludes": [
    "**/monorepo/CLAUDE.md",
    "/home/user/monorepo/other-team/.claude/rules/**"
  ]
}
```

Patterns are matched against absolute file paths using glob syntax. You can configure `claudeMdExcludes` at any [settings layer](/en/settings#settings-files): user, project, local, or managed policy. Arrays merge across layers.

模式使用 glob 语法与绝对文件路径匹配。你可以在任意[设置层级](/en/settings#settings-files)配置 `claudeMdExcludes`：用户、项目、本地或托管策略。数组会跨层级合并。

Managed policy CLAUDE.md files cannot be excluded. This ensures organization-wide instructions always apply regardless of individual settings.

托管策略 CLAUDE.md 文件无法被排除。这确保组织级指令始终生效，不受个人设置影响。

### Auto memory

Auto memory lets Claude accumulate knowledge across sessions without you writing anything. Claude saves notes for itself as it works: build commands, debugging insights, architecture notes, code style preferences, and workflow habits. Claude doesn't save something every session. It decides what's worth remembering based on whether the information would be useful in a future conversation.

### Auto memory

auto memory 让 Claude 无需你编写任何内容即可跨会话积累知识。Claude 在工作中为自己保存笔记：构建命令、调试经验、架构笔记、代码风格偏好以及工作流习惯。Claude 并非每次会话都会保存内容。它会根据信息在将来对话中是否有用来决定是否值得记忆。

Auto memory requires Claude Code v2.1.59 or later. Check your version with `claude --version`.

auto memory 需要 Claude Code v2.1.59 或更高版本。用 `claude --version` 检查你的版本。

### Enable or disable auto memory

Auto memory is on by default. To toggle it, open `/memory` in a session and use the auto memory toggle, or set `autoMemoryEnabled` in your project settings:

```json theme={null}
{
  "autoMemoryEnabled": false
}
```

### 启用或禁用 auto memory

auto memory 默认开启。要切换状态，请在会话中打开 `/memory` 并使用 auto memory 开关，或在项目设置中设置 `autoMemoryEnabled`：

```json theme={null}
{
  "autoMemoryEnabled": false
}
```

To disable auto memory via environment variable, set `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`.

要通过环境变量禁用 auto memory，请设置 `CLAUDE_CODE_DISABLE_AUTO_MEMORY=1`。

### Storage location

Each project gets its own memory directory at `~/.claude/projects/<project>/memory/`. The `<project>` path is derived from the git repository, so all worktrees and subdirectories within the same repo share one auto memory directory. Outside a git repo, the project root is used instead.

### 存储位置

每个项目拥有自己的记忆目录 `~/.claude/projects/<project>/memory/`。`<project>` 路径由 git 仓库派生，因此同一仓库内的所有 worktree 和子目录共享一个 auto memory 目录。在 git 仓库之外，则使用项目根目录。

To store auto memory in a different location, set `autoMemoryDirectory` in your `settings.json`. It is read from any [settings scope](/en/settings#settings-precedence): user, project, local, policy, or `--settings`.

```json theme={null}
{
  "autoMemoryDirectory": "~/my-custom-memory-dir"
}
```

若要将 auto memory 存储到其他位置，请在 `settings.json` 中设置 `autoMemoryDirectory`。它可从任意[设置作用域](/en/settings#settings-precedence)读取：用户、项目、本地、策略或 `--settings`。

```json theme={null}
{
  "autoMemoryDirectory": "~/my-custom-memory-dir"
}
```

The value must be an absolute path or start with `~/`. When set in a project's `.claude/settings.json` or `.claude/settings.local.json`, the value is honored only after you accept the workspace trust dialog for that folder, the same gate that governs hooks.

该值必须是绝对路径或以 `~/` 开头。当在项目的 `.claude/settings.json` 或 `.claude/settings.local.json` 中设置时，该值只有在你就该文件夹接受工作区信任对话框后才会生效，这与管理 hooks 的门控相同。

The directory contains a `MEMORY.md` entrypoint and optional topic files:

```text theme={null}
~/.claude/projects/<project>/memory/
├── MEMORY.md          # Concise index, loaded into every session
├── debugging.md       # Detailed notes on debugging patterns
├── api-conventions.md # API design decisions
└── ...                # Any other topic files Claude creates
```

该目录包含一个 `MEMORY.md` 入口文件和可选的主题文件：

```text theme={null}
~/.claude/projects/<project>/memory/
├── MEMORY.md          # Concise index, loaded into every session
├── debugging.md       # Detailed notes on debugging patterns
├── api-conventions.md # API design decisions
└── ...                # Any other topic files Claude creates
```

`MEMORY.md` acts as an index of the memory directory. Claude reads and writes files in this directory throughout your session, using `MEMORY.md` to keep track of what's stored where.

`MEMORY.md` 充当记忆目录的索引。Claude 在整个会话过程中读写该目录中的文件，并使用 `MEMORY.md` 记录各内容存储位置。

Auto memory is machine-local. All worktrees and subdirectories within the same git repository share one auto memory directory. Files are not shared across machines or cloud environments.

auto memory 是机器本地的。同一 git 仓库内的所有 worktree 和子目录共享一个 auto memory 目录。文件不会跨机器或云环境共享。

### How it works

The first 200 lines of `MEMORY.md`, or the first 25KB, whichever comes first, are loaded at the start of every conversation. Content beyond that threshold is not loaded at session start. Claude keeps `MEMORY.md` concise by moving detailed notes into separate topic files.

### 工作原理

每次对话开始时，会加载 `MEMORY.md` 的前 200 行或前 25KB（以先到者为准）。超出该阈值的内容不会在会话开始时加载。Claude 通过将详细笔记移入单独的主题文件来保持 `MEMORY.md` 简洁。

This limit applies only to `MEMORY.md`. CLAUDE.md files are loaded in full regardless of length, though shorter files produce better adherence.

此限制仅适用于 `MEMORY.md`。CLAUDE.md 文件无论多长都会完整加载，但较短的文件遵循度更好。

Topic files like `debugging.md` or `patterns.md` are not loaded at startup. Claude reads them on demand using its standard file tools when it needs the information.

`debugging.md` 或 `patterns.md` 等主题文件不会在启动时加载。Claude 会在需要时使用其标准文件工具按需读取。

Claude reads and writes memory files during your session. When you see "Writing memory" or "Recalled memory" in the Claude Code interface, Claude is actively updating or reading from `~/.claude/projects/<project>/memory/`.

Claude 在会话期间读写记忆文件。当你在 Claude Code 界面看到"Writing memory"或"Recalled memory"时，表示 Claude 正在更新或读取 `~/.claude/projects/<project>/memory/`。

### Audit and edit your memory

Auto memory files are plain markdown you can edit or delete at any time. Run [`/memory`](#view-and-edit-with-%2Fmemory) to browse and open memory files from within a session.

### 审计和编辑你的记忆

auto memory 文件是普通 markdown，你可以随时编辑或删除。运行 [`/memory`](#view-and-edit-with-%2Fmemory) 可在会话中浏览并打开记忆文件。

### View and edit with `/memory`

The `/memory` command lists all CLAUDE.md, CLAUDE.local.md, and rules files loaded in your current session, lets you toggle auto memory on or off, and provides a link to open the auto memory folder. Select any file to open it in your editor.

### 使用 `/memory` 查看和编辑

`/memory` 命令会列出当前会话中加载的所有 CLAUDE.md、CLAUDE.local.md 和 rules 文件，允许你开启或关闭 auto memory，并提供打开 auto memory 文件夹的链接。选择任意文件即可在编辑器中打开。

When you ask Claude to remember something, like "always use pnpm, not npm" or "remember that the API tests require a local Redis instance," Claude saves it to auto memory. To add instructions to CLAUDE.md instead, ask Claude directly, like "add this to CLAUDE.md," or edit the file yourself via `/memory`.

当你要求 Claude 记住某些内容时，比如"始终使用 pnpm，而非 npm"或"记住 API 测试需要本地 Redis 实例"，Claude 会将其保存到 auto memory。若要改为向 CLAUDE.md 添加指令，请直接告诉 Claude，如"把这条加到 CLAUDE.md"，或通过 `/memory` 自行编辑文件。

### Troubleshoot memory issues

These are the most common issues with CLAUDE.md and auto memory, along with steps to debug them.

### 记忆问题故障排查

以下是 CLAUDE.md 和 auto memory 最常见的问题，以及调试步骤。

### Claude isn't following my CLAUDE.md

CLAUDE.md content is delivered as a user message after the system prompt, not as part of the system prompt itself. Claude reads it and tries to follow it, but there's no guarantee of strict compliance, especially for vague or conflicting instructions.

### Claude 不遵循我的 CLAUDE.md

CLAUDE.md 内容作为系统提示之后的用户消息传递，而非系统提示本身的一部分。Claude 会读取并尝试遵循，但无法保证严格遵从，尤其对于模糊或冲突的指令。

To debug:

* Run `/memory` to verify your CLAUDE.md and CLAUDE.local.md files are being loaded. If a file isn't listed, Claude can't see it.
* Check that the relevant CLAUDE.md is in a location that gets loaded for your session (see [Choose where to put CLAUDE.md files](#choose-where-to-put-claude-md-files)).
* Make instructions more specific. "Use 2-space indentation" works better than "format code nicely."
* Look for conflicting instructions across CLAUDE.md files. If two files give different guidance for the same behavior, Claude may pick one arbitrarily.

调试方法：

* 运行 `/memory` 验证你的 CLAUDE.md 和 CLAUDE.local.md 文件是否被加载。若某文件未被列出，Claude 就看不到它。
* 检查相关 CLAUDE.md 是否位于会话会加载的位置（见[选择 CLAUDE.md 文件的放置位置](#choose-where-to-put-claude-md-files)）。
* 让指令更具体。"使用 2 空格缩进"比"把代码格式化好"更有效。
* 检查各 CLAUDE.md 文件间是否存在冲突指令。如果两个文件对同一行为给出不同指引，Claude 可能任意选择其一。

If the instruction is something that must run at a specific point, such as before every commit or after each file edit, write it as a [hook](/en/hooks-guide) instead. Hooks execute as shell commands at fixed lifecycle events and apply regardless of what Claude decides to do.

如果指令是必须在特定时机执行的内容，例如每次提交前或每次文件编辑后，请改为编写为 [hook](/en/hooks-guide)。hooks 作为 shell 命令在固定的生命周期事件中执行，无论 Claude 如何决策都会生效。

For instructions you want at the system prompt level, use [`--append-system-prompt`](/en/cli-reference#system-prompt-flags). This must be passed every invocation, so it's better suited to scripts and automation than interactive use.

对于希望放在系统提示层级的指令，请使用 [`--append-system-prompt`](/en/cli-reference#system-prompt-flags)。这必须在每次调用时传入，因此更适合脚本和自动化而非交互式使用。

Use the [`InstructionsLoaded` hook](/en/hooks#instructionsloaded) to log exactly which instruction files are loaded, when they load, and why. This is useful for debugging path-specific rules or lazy-loaded files in subdirectories.

使用 [`InstructionsLoaded` hook](/en/hooks#instructionsloaded) 记录确切加载了哪些指令文件、何时加载以及为何加载。这对调试路径限定规则或子目录中的懒加载文件很有用。

### I don't know what auto memory saved

Run `/memory` and select the auto memory folder to browse what Claude has saved. Everything is plain markdown you can read, edit, or delete.

### 我不知道 auto memory 保存了什么

运行 `/memory` 并选择 auto memory 文件夹，浏览 Claude 已保存的内容。所有内容都是普通 markdown，可阅读、编辑或删除。

### My CLAUDE.md is too large

Files over 200 lines consume more context and may reduce adherence. Use [path-scoped rules](#path-specific-rules) to load instructions only when Claude works with matching files, or trim content that isn't needed in every session. Splitting into [`@path` imports](#import-additional-files) helps organization but does not reduce context, since imported files load at launch.

### 我的 CLAUDE.md 太大

超过 200 行的文件会消耗更多上下文并可能降低遵循度。使用[路径限定规则](#path-specific-rules)让指令只在 Claude 处理匹配文件时加载，或裁剪并非每次会话都需要的内容。拆分为 [`@path` 导入](#import-additional-files)有助于组织，但不会减少上下文，因为导入的文件在启动时就会加载。

### Instructions seem lost after `/compact`

Project-root CLAUDE.md survives compaction: after `/compact`, Claude re-reads it from disk and re-injects it into the session. Nested CLAUDE.md files in subdirectories are not re-injected automatically; they reload the next time Claude reads a file in that subdirectory.

### `/compact` 后指令似乎丢失了

项目根目录的 CLAUDE.md 能在压缩后保留：执行 `/compact` 后，Claude 会从磁盘重新读取并将其重新注入会话。子目录中的嵌套 CLAUDE.md 文件不会自动重新注入；它们会在 Claude 下次读取该子目录中的文件时重新加载。

If an instruction disappeared after compaction, it was either given only in conversation or lives in a nested CLAUDE.md that hasn't reloaded yet. Add conversation-only instructions to CLAUDE.md to make them persist. See [What survives compaction](/en/context-window#what-survives-compaction) for the full breakdown.

如果某条指令在压缩后消失，它要么仅存在于对话中，要么位于尚未重新加载的嵌套 CLAUDE.md 中。将仅存在于对话中的指令添加到 CLAUDE.md 使其持久化。完整说明请参阅[压缩后保留的内容](/en/context-window#what-survives-compaction)。

See [Write effective instructions](#write-effective-instructions) for guidance on size, structure, and specificity.

关于篇幅、结构和具体性的指引，请参阅[编写有效的指令](#write-effective-instructions)。

### Related resources

* [Debug your configuration](/en/debug-your-config): diagnose why CLAUDE.md or settings aren't taking effect
* [Skills](/en/skills): package repeatable workflows that load on demand
* [Settings](/en/settings): configure Claude Code behavior with settings files
* [Subagent memory](/en/sub-agents#enable-persistent-memory): let subagents maintain their own auto memory

### 相关资源

* [调试你的配置](/en/debug-your-config)：诊断 CLAUDE.md 或设置为何未生效
* [Skills](/en/skills)：打包可按需加载的重复性工作流
* [Settings](/en/settings)：通过设置文件配置 Claude Code 行为
* [Subagent 记忆](/en/sub-agents#enable-persistent-memory)：让 subagent 维护各自的 auto memory


---

## Claude Code - Power user tips（进阶用户技巧）

原文链接：https://support.claude.com/en/articles/14554000-claude-code-power-user-tips

### Claude Code power user tips

This article collects workflow tips from the Claude Code team at Anthropic. These practices cover parallel execution, planning, automation, verification, and customization—the patterns the team uses every day to ship code faster. Everyone's setup is different, so experiment to see what works for you.

本文汇集了 Anthropic Claude Code 团队的工作流 (workflow，即日常工作流程) 实践技巧。这些实践涵盖并行执行、规划、自动化、验证与自定义——都是团队每天用来更快交付代码的模式。每个人的配置不同，请多加试验，找到最适合自己的方式。

**Important:** The single most impactful tip in this guide is **verification**—giving Claude a way to check its own output. If you only adopt one practice, make it that one. See the **[Verification](#h_7dd53c5c29)** section below.

**重要提示：** 本指南中影响最大的一条技巧是**验证**——即给 Claude 一种检查自身输出的方式。如果你只采纳一个实践，就选这一条。参见下文的 **[Verification（验证）](#h_7dd53c5c29)** 章节。

**Before you start: scope of this guide**

**开始之前：本指南的范围**

These are power-user patterns collected from individual engineers on the Claude Code team. As a result:

这些是来自 Claude Code 团队各位工程师的进阶用户模式。因此：

-   Most commands shown here ship with Claude Code: /color and /btw are **built-in commands**, and /simplify and /loop are **bundled skills** that ship with the CLI. See the **[commands reference](https://code.claude.com/docs/en/commands)** and **[skills](https://code.claude.com/docs/en/skills)**. You can build your own skills by adding a SKILL.md file under .claude/skills/<name>/.

-   此处展示的大多数命令随 Claude Code 一起发布：/color 和 /btw 是**内置命令**，而 /simplify 和 /loop 是随 CLI 一起发布的**内置 skill（技能，可复用的工作流封装）**。参见 **[命令参考](https://code.claude.com/docs/en/commands)** 与 **[skills](https://code.claude.com/docs/en/skills)**。你可以通过在 .claude/skills/<name>/ 下添加一个 SKILL.md 文件来构建自己的 skill。

-   The iMessage plugin ships in the official claude-plugins-official marketplace. Community plugins (for example the "ralph-wiggum" plugin) are not reviewed or sanctioned by Anthropic — check with your administrator before installing third-party plugins in a managed environment.

-   iMessage 插件随官方 claude-plugins-official 插件市场发布。社区插件（例如 "ralph-wiggum" 插件）未经 Anthropic 审查或认可——在受管环境中安装第三方插件前，请先咨询你的管理员。

-   Some capabilities—auto mode, sandboxing, remote control, scheduled cloud jobs, voice—are **off by default** and may be disabled by your organization's policy. If a command or flag here returns "not available," your admin has likely not enabled it for your workspace.

-   部分能力——自动模式、沙箱 (sandbox，一种隔离运行环境)、远程控制、云端定时任务、语音——**默认关闭**，且可能被你所在组织的策略禁用。如果此处的某个命令或参数返回 "not available"，那很可能是你的管理员尚未为你的工作区启用它。

Everything else in this guide works on a stock Claude Code install. When in doubt, run `/help` to see what is actually available in your session.

本指南中的其他内容在全新安装的 Claude Code 上都能正常使用。如有疑问，请运行 `/help` 查看你的会话中实际可用的功能。

### Contents

**Section** | **Covers**

### 目录

**章节** | **涵盖内容**

Working in Parallel | Worktrees, subagent isolation, `/batch`

并行工作 | worktree、subagent 隔离、`/batch`

Planning Before Building | Plan mode, model choice, effort levels

构建前规划 | Plan 模式、模型选择、effort 等级

Prompting Effectively | Pushback prompts, `/btw`

高效提示 | 反问式 prompt、`/btw`

Learning With Claude | Explanatory output, diagrams, spaced repetition

与 Claude 一起学习 | 解释性输出、图表、间隔重复

CLAUDE.md and Memory | `/init`, @claude in PRs, auto-memory, auto-dream

CLAUDE.md 与记忆 | `/init`、PR 中的 @claude、自动记忆、auto-dream

Verification | Chrome extension, Desktop app, `/simplify`

验证 | Chrome 扩展、桌面应用、`/simplify`

Commands, Skills, and Subagents | Custom commands, agent definitions, code-review agents

命令、skill 与 subagent | 自定义命令、agent 定义、code review（代码审查）agent

Hooks | Lifecycle events and patterns

Hook | 生命周期事件与模式

Permissions and Safety | Pre-approvals, auto mode, sandboxing, long-running tasks

权限与安全 | 预先批准、自动模式、沙箱、长时间运行的任务

Scheduled and Recurring Tasks | `/loop`, `/schedule`

定时与循环任务 | `/loop`、`/schedule`

Mobile and Remote Control | Mobile app, teleport, remote control, Dispatch

移动端与远程控制 | 移动应用、teleport、远程控制、Dispatch

Tool Integrations (MCP) | Data analytics, bug fixing, plugins

工具集成 (MCP) | 数据分析、bug 修复、插件

Customizing Your Environment | Terminal, status line, voice, output styles

自定义你的环境 | 终端、状态栏、语音、输出样式

SDK and Multi-Repo Work | `--bare`, `--add-dir`, forking, setup scripts

SDK 与多仓库工作 | `--bare`、`--add-dir`、fork、setup 脚本

### Working in parallel

### 并行工作

#### Run multiple sessions at once

The biggest productivity unlock is running 3–5 Claude sessions in parallel, each in its own git worktree. Claude Code has native worktree support built in.

#### 同时运行多个会话

最大的生产力解锁方式是并行运行 3–5 个 Claude 会话，每个会话使用各自的 git worktree。Claude Code 内置了原生 worktree 支持。

-   From the CLI, run `claude --worktree` (or `claude --worktree my_worktree`) to start a session in an isolated worktree. Add `--tmux` to launch in its own Tmux session.

-   在 CLI 中，运行 `claude --worktree`（或 `claude --worktree my_worktree`）在一个隔离的 worktree 中启动会话。加上 `--tmux` 可在其独立的 Tmux 会话中启动。

-   From the Desktop app, open the Code tab and check the worktree checkbox.

-   在桌面应用中，打开 Code 标签页并勾选 worktree 复选框。

-   For non-git VCS (Mercurial, Perforce, SVN), define `WorktreeCreate` and `WorktreeRemove` hooks in your `settings.json` to get the same isolation.

-   对于非 git 的 VCS（Mercurial、Perforce、SVN），在 `settings.json` 中定义 `WorktreeCreate` 和 `WorktreeRemove` hook 即可获得相同的隔离能力。

To stay oriented across many sessions, name your worktrees, set up shell aliases (`za`, `zb`, `zc`) to jump between them, color-code your terminal tabs, and enable terminal notifications so you know when any Claude needs your attention. Many engineers keep a dedicated "analysis" worktree just for reading logs and running queries.

要在多个会话之间保持清晰，可以给 worktree 命名、设置 shell 别名（`za`、`zb`、`zc`）在它们之间快速跳转、为终端标签页用颜色区分，并启用终端通知，这样当某个 Claude 需要你关注时你能及时知晓。许多工程师会专门保留一个"分析"worktree，只用于查看日志和运行查询。

#### Subagents with worktree isolation

Subagents can also run in isolated worktrees, which is especially powerful for large batched changes. Add `isolation: worktree` to your agent's frontmatter:

#### 带 worktree 隔离的 subagent

subagent（子代理，由主 agent 派生的从属代理）也可以在隔离的 worktree 中运行，这对于大批量变更尤其强大。在你的 agent 的 frontmatter 中添加 `isolation: worktree`：

```
# .claude/agents/worktree-worker.md
---
name: worktree-worker
model: haiku
isolation: worktree
---
```

Then prompt naturally: *"Migrate all sync IO to async. Batch the changes and launch 10 parallel agents with worktree isolation. Each agent should test its changes end to end, then put up a PR."*

然后自然地输入 prompt（提示词，发给模型的指令）：*"将所有同步 IO 迁移为异步。批量处理这些变更，并启动 10 个带 worktree 隔离的并行 agent。每个 agent 应端到端测试自己的变更，然后提交一个 PR（pull request，代码合并请求）。"*

#### /batch for large migrations

The `/batch` command interviews you about a migration, then fans the work out to as many worktree agents as needed — dozens, hundreds, or more. Each agent works in isolation, tests its own changes, and creates a PR independently.

#### 用于大型迁移的 /batch

`/batch` 命令会先就一次迁移向你询问，然后将工作分发给所需数量的 worktree agent——几十个、几百个甚至更多。每个 agent 独立隔离工作，测试自己的变更，并独立创建一个 PR。

```
> /batch migrate src/ from Solid to React
```

### Planning before building

### 构建前规划

#### Start complex tasks in plan mode

Press **Shift+Tab** to cycle into plan mode. Pour your effort into the plan so Claude can one-shot the implementation. The typical flow is: enter plan mode → refine the plan → switch to auto-accept edits → Claude executes.

#### 在 plan 模式中开始复杂任务

按 **Shift+Tab** 循环切换到 plan（规划）模式。把精力投入到规划中，这样 Claude 就能一次性完成实现。典型流程是：进入 plan 模式 → 完善规划 → 切换到自动接受编辑 → Claude 执行。

A few patterns from the team:

以下是团队的一些模式：

-   Have one Claude write a plan, then spin up a second Claude to review it as a staff engineer.

-   让一个 Claude 写规划，然后启动第二个 Claude 以资深工程师的身份来审查它。

-   The moment something goes sideways, switch back to plan mode and re-plan rather than course-correcting mid-stream.

-   一旦出现偏差，就切回 plan 模式重新规划，而不是在中途边走边修。

-   After plan mode, Claude **automatically names your session** based on what you're working on—you can also set a name upfront with `claude --name "auth-refactor"`.

-   plan 模式结束后，Claude 会根据你正在做的事**自动为会话命名**——你也可以用 `claude --name "auth-refactor"` 提前设定一个名字。

#### Use Opus with thinking for everything

Claude Code team's reasoning: *"It's the best coding model I've ever used, and even though it's bigger & slower than Sonnet, since you have to steer it less and it's better at tool use, it is almost always faster than using a smaller model in the end."*

#### 一切任务都使用带 thinking 的 Opus

Claude Code 团队的理由：*"这是我用过的最好的编程模型，尽管它比 Sonnet 更大也更慢，但因为你需要给它的引导更少、它对工具的使用更出色，最终几乎总是比用更小的模型更快。"*

**The math:** less steering + better tool use = faster overall results, even with a larger model.

**算一笔账：** 更少的引导 + 更好的工具使用 = 更快的整体结果，即便用的是更大的模型。

#### Effort level

Run /effort to choose your effort level. The available levels are **low** (fewer tokens, faster), **medium**, **high** (more tokens, more intelligence), **xhigh**, **max**, and **auto** (Claude chooses per request). The default is **high** on Team, Enterprise, and direct API access, and **medium** on other plans. The Claude Code team uses high for everything. For complex coding and agentic work, switch to xhigh for deeper reasoning than high without the full token cost of max. Switch to max for hard debugging or architecture decisions where you want Claude to reason for as long as it needs. Max burns through usage limits faster, so activate it per session.

#### effort 等级

运行 /effort 来选择你的 effort（投入）等级。可用等级包括 **low**（更少 token、更快）、**medium**、**high**（更多 token、更智能）、**xhigh**、**max**，以及 **auto**（由 Claude 按请求选择）。默认值在 Team、Enterprise 和直连 API 访问中为 **high**，在其他套餐中为 **medium**。Claude Code 团队对所有任务都用 high。对于复杂编程和 agentic（具备自主行动能力的）工作，切到 xhigh 可获得比 high 更深的推理，而又不像 max 那样消耗全部 token 成本。当遇到困难调试或架构决策、希望 Claude 尽情推理时，切到 max。max 会更快耗尽用量上限，因此建议按会话启用。

### Prompting effectively

Don't accept the first solution—push Claude to do better. A few prompts that work well:

### 高效提示

不要直接接受第一个方案——推动 Claude 做得更好。以下是几个行之有效的 prompt：

-   **"Grill me on these changes and don't make a PR until I pass your test."** Forces Claude to validate your understanding before shipping.

-   **"就这些变更狠狠拷问我，在我通过你的测试之前不要提 PR。"** 迫使 Claude 在交付前验证你是否真的理解。

-   **"Prove to me this works."** Have Claude diff behavior between `main` and your feature branch.

-   **"向我证明这是可行的。"** 让 Claude 对比 `main` 与你的 feature 分支之间的行为差异。

-   **"Knowing everything you know now, scrap this and implement the elegant solution."** Useful after a mediocre first attempt.

-   **"基于你现在掌握的一切，推翻重做，实现那个优雅的方案。"** 在第一次尝试平庸时尤其有用。

Write detailed specs to reduce ambiguity before handing work off. The more specific you are, the better the output.

在把工作交给 Claude 之前，先写详细的规格说明以减少歧义。你写得越具体，输出就越好。

#### /btw for side questions

While Claude is actively working, use `/btw` to ask a quick question without interrupting it. It's single-turn with no tool calls, but has full context of the conversation.

#### 用于旁路提问的 /btw

当 Claude 正在工作中时，用 `/btw` 提一个快速问题而不会打断它。它是单轮、不调用工具的，但拥有对话的完整上下文。

```
> /btw what does the retry logic do?
```

### Learning with Claude

Claude Code isn't just for writing code—it's a powerful learning tool when you configure it to explain and teach.

### 与 Claude 一起学习

Claude Code 不仅仅是写代码的工具——当你把它配置成讲解和教学的助手时，它是一个强大的学习工具。

-   **Enable "Explanatory" or "Learning" output style** in `/config` to have Claude explain the *why* behind changes.

-   在 `/config` 中启用 **"Explanatory"（解释性）或 "Learning"（学习型）输出样式**，让 Claude 解释变更背后的*原因*。

-   **Generate visual HTML presentations** explaining unfamiliar code.

-   **生成可视化的 HTML 演示**来解释陌生的代码。

-   **Ask for ASCII diagrams** of new protocols and codebases.

-   **请求 ASCII 图表**来呈现新的协议和代码库。

-   **Build a spaced-repetition skill:** explain your understanding, Claude asks follow-ups to fill gaps.

-   **构建一个间隔重复 (spaced-repetition) skill：** 讲解你的理解，由 Claude 提出追问来填补知识空白。

### CLAUDE.md and memory

### CLAUDE.md 与记忆

#### Invest in your CLAUDE.md

Share a single `CLAUDE.md` file at your repo root, checked into git, with the whole team contributing. The key practice: **anytime Claude does something incorrectly, add it to CLAUDE.md** so it knows not to repeat the mistake.

#### 投入精力到你的 CLAUDE.md

在仓库根目录共享一个 `CLAUDE.md` 文件，提交到 git，由整个团队共同维护。关键实践是：**每当 Claude 做错了某件事，就把它加进 CLAUDE.md**，让它知道不要再犯同样的错误。

After every correction, end with: *"Update your CLAUDE.md so you don't make that mistake again."* Claude is very good at writing rules for itself.

每次纠正之后，以这句话收尾：*"更新你的 CLAUDE.md，这样你就不会再犯那个错误了。"* Claude 非常擅长为自己编写规则。

#### @claude in Code Reviews

Install the GitHub Action with `/install-github-app`, then tag `@claude` in PR comments to add learnings to `CLAUDE.md` as part of the review:

#### code review 中的 @claude

用 `/install-github-app` 安装 GitHub Action，然后在 PR 评论中标记 `@claude`，作为 code review 的一部分将经验沉淀进 `CLAUDE.md`：

```
nit: use a string literal, not ts enum
@claude add to CLAUDE.md to never use enums, always prefer literal unions
```

This is "Compounding Engineering"—each correction makes every future session better.

这就是"复利式工程"——每一次纠正都让此后的每一次会话变得更好。

#### Auto-memory

Run `/memory` to configure Claude Code's built-in memory system.

#### 自动记忆

运行 `/memory` 来配置 Claude Code 内置的记忆系统。

**Auto-memory** automatically saves preferences, corrections, and patterns between sessions. Memories are written to ~/.claude/projects/<project>/memory/ (one directory per git repo root). This is separate from your user-level ~/.claude/CLAUDE.md and project-level ./CLAUDE.md files, which you maintain by hand.

**Auto-memory（自动记忆）** 会在会话之间自动保存偏好、纠正和模式。记忆被写入 ~/.claude/projects/<project>/memory/（每个 git 仓库根目录对应一个目录）。它与用户级 ~/.claude/CLAUDE.md 和项目级 ./CLAUDE.md 文件相互独立——后者由你手动维护。

The naming maps to how REM sleep consolidates short-term memory into long-term storage.

其命名映射的是 REM 睡眠如何将短期记忆固化为长期存储的过程。

#### Advanced: Notes directory

One engineer on the team tells Claude to maintain a notes directory for every task and project, updated after every PR — then points `CLAUDE.md` at it.

#### 进阶：Notes 目录

团队中的一位工程师让 Claude 为每个任务和项目维护一个 notes 目录，在每次 PR 后更新——然后让 `CLAUDE.md` 指向它。

### Verification — the #1 Tip

Giving Claude a way to verify its work will markedly improve the quality of the final result. If Claude can close the feedback loop on its own, it will iterate until the output is right.

### 验证——第一技巧

给 Claude 一种验证自身工作的方式，将显著提升最终结果的质量。如果 Claude 能自行闭合反馈回路，它就会不断迭代，直到输出正确为止。

Verification looks different per domain—bash commands, test suites, simulators, browser testing—but the principle is the same. Invest in domain-specific verification.

不同领域的验证形式各异——bash 命令、测试套件、模拟器、浏览器测试——但原则相同。请为你的领域投入专门的验证手段。

#### The Chrome extension

For frontend work, install the Claude Code Chrome extension. Think of it like any other engineer: if you ask someone to build a website but don't let them use a browser, will it look good? Probably not. With a browser, they'll iterate until it does.

#### Chrome 扩展

对于前端工作，请安装 Claude Code 的 Chrome 扩展。可以把它想象成任何一位工程师：如果你让某人做一个网站却不让他用浏览器，结果会好看吗？大概率不会。有了浏览器，他就会不断迭代直到做出来。

The team uses the Chrome extension every time they work on web code. Download it for Chrome or Edge at **[code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome)**.

团队每次写 web 代码都会用 Chrome 扩展。可在 **[code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome)** 下载 Chrome 或 Edge 版本。

#### Desktop app for web servers

The Claude Desktop app bundles the ability to **automatically start and test web servers** in a built-in browser. You can set up something similar in CLI or VS Code using the Chrome extension, or just use the Desktop app directly.

#### 用于 web 服务器的桌面应用

Claude 桌面应用内置了**自动启动并测试 web 服务器**的能力，使用其内置浏览器。你可以在 CLI 或 VS Code 中通过 Chrome 扩展实现类似效果，或者直接使用桌面应用。

#### /simplify for Code Quality

Append `/simplify` to any prompt after making changes. It runs parallel agents that review changed code for reuse, quality, efficiency, and `CLAUDE.md` compliance—all in one pass.

#### 用于代码质量的 /simplify

在做完变更后，把 `/simplify` 追加到任意 prompt 后面。它会并行运行多个 agent，一次性从复用性、质量、效率以及 `CLAUDE.md` 合规性等方面审查已变更的代码。

```
> hey claude make this code change then run /simplify
```

### Commands, skills, and subagents

### 命令、skill 与 subagent

#### Skills for repeated workflows

If you do something more than once a day, turn it into a skill. Skills are checked into `.claude/skills/<name>/SKILL.md` and shared with the team (the legacy `.claude/commands/` path still works, but skills are the recommended approach). A few ideas:

#### 用于重复工作流的 skill

如果你某件事一天要做不止一次，就把它做成一个 skill。skill 存放在 `.claude/skills/<name>/SKILL.md` 中并提交到 git 与团队共享（旧版的 `.claude/commands/` 路径仍然可用，但推荐使用 skill）。几个思路：

-   A `/techdebt` command that runs at the end of every session to find duplicated code.

-   一个 `/techdebt` 命令，在每次会话结束时运行，用于查找重复代码。

-   A command that syncs 7 days of Slack, GDrive, Asana, and GitHub into one context dump.

-   一个命令，把最近 7 天的 Slack、GDrive、Asana 和 GitHub 内容同步成一份上下文汇总。

-   Analytics-engineer agents that write dbt models, review code, and test in dev.

-   分析工程师 agent，负责编写 dbt 模型、审查代码并在 dev 环境中测试。

Slash commands can include **inline Bash** to pre-compute info (like `git status`) without extra model calls.

斜杠命令可以包含**内联 Bash**，用于在不额外调用模型的情况下预先计算信息（例如 `git status`）。

#### Subagents for PR workflows

Think of subagents as automations for your most common PR workflows. Drop `.md` files into `.claude/agents/`:

#### 用于 PR 工作流的 subagent

把 subagent 想象成你最常用的 PR 工作流的自动化。把 `.md` 文件放进 `.claude/agents/`：

```
.claude/agents/
  build-validator.md
  code-architect.md
  code-simplifier.md
  verify-app.md
```

Each agent can have a custom name, color, tool set, allowed/disallowed tools, permission mode, and model. Set the **default agent for your main conversation** by adding `"agent"` to `settings.json` or using `claude --agent <name>`. Run `/agents` to get started.

每个 agent 都可以有自己的名称、颜色、工具集、允许/禁止的工具、权限模式和模型。通过在 `settings.json` 中添加 `"agent"` 或使用 `claude --agent <name>` 来**为你的主对话设置默认 agent**。运行 `/agents` 即可上手。

#### --agent for custom system prompts

Custom agents are a powerful primitive that often gets overlooked. Define a new agent in `.claude/agents`, then run `claude --agent=<name>`. Example of a read-only agent:

#### 用于自定义系统 prompt 的 --agent

自定义 agent 是一个常被忽视的强大原语。在 `.claude/agents` 中定义一个新 agent，然后运行 `claude --agent=<name>`。下面是一个只读 agent 的示例：

```
# .claude/agents/ReadOnly.md
---
name: ReadOnly
description: Read-only agent restricted to the Read tool only
color: blue
tools: Read
---
You are a read-only agent that cannot edit files or run bash.
```

#### Leveraging subagents at runtime

-   **Append "use subagents"** to any request where you want Claude to throw more compute at the problem.

#### 在运行时利用 subagent

-   **在任何请求后追加 "use subagents"**，当你希望 Claude 对该问题投入更多算力时。

-   **Offload individual tasks to subagents** to keep your main agent's context window clean and focused.

-   **把单个任务下发给 subagent**，以保持主 agent 的上下文窗口干净且专注。

-   **Route permission requests to Opus via a hook** — let it scan for attacks and auto-approve the safe ones.

-   **通过 hook 将权限请求转给 Opus** —— 让它扫描攻击并自动批准安全请求。

#### Code review agents

When a PR opens, Claude can dispatch a team of agents that each focus on a different concern — logic errors, security issues, performance regressions — and post inline comments. The Anthropic team built this for themselves first; code output per engineer increased significantly and reviews were the bottleneck.

#### code review agent

当一个 PR 创建时，Claude 可以派出一组 agent，每个 agent 专注于不同方面——逻辑错误、安全问题、性能回归——并发布行内评论。Anthropic 团队最先为自己构建了这套机制；每位工程师的代码产出显著提升，而 code review 不再是瓶颈。

### Hooks

Hooks let you deterministically run logic at points in the agent lifecycle. Ask Claude to add a hook to get started.

### Hook

hook（钩子，在 agent 生命周期特定时点确定性地执行逻辑）让你可以在 agent 生命周期的各个时点确定性地运行逻辑。请 Claude 帮你添加一个 hook 即可上手。

#### Common hook patterns

**Event** | **Use case**

#### 常见 hook 模式

**事件** | **用途**

`SessionStart` | Dynamically load context each time you start Claude

`SessionStart` | 每次启动 Claude 时动态加载上下文

`PreToolUse` | Log every bash command the model runs

`PreToolUse` | 记录模型运行的每一条 bash 命令

`PostToolUse` | Auto-format code after Write/Edit to prevent CI failures

`PostToolUse` | 在 Write/Edit 之后自动格式化代码，以防 CI（持续集成）失败

`PermissionRequest` | Route permission prompts to Slack, WhatsApp, or Opus for review

`PermissionRequest` | 将权限提示转给 Slack、WhatsApp 或 Opus 审查

`Stop` | Run deterministic checks on long tasks, or nudge Claude to keep going

`Stop` | 对长任务运行确定性检查，或推动 Claude 继续工作

`PostCompact` | Re-inject critical instructions after context compression

`PostCompact` | 在上下文压缩后重新注入关键指令

Example `PostToolUse` hook for auto-formatting:

用于自动格式化的 `PostToolUse` hook 示例：

```
"PostToolUse": [
  {
    "matcher": "Write|Edit",
    "hooks": [{ "type": "command", "command": "bun run format || true" }]
  }
]
```

### Permissions and safety

### 权限与安全

#### Pre-approve common commands

Run `/permissions` to pre-allow common safe commands and check them into your team's `.claude/settings.json`. This is the **recommended alternative** to skipping permissions entirely — you get fewer prompts while keeping an auditable allowlist. **Full wildcard syntax is supported**—try `"Bash(bun run *)"` or `"Edit(/docs/**)"`.

#### 预先批准常用命令

运行 `/permissions` 预先允许常用的安全命令，并将它们提交到团队的 `.claude/settings.json` 中。这是**推荐的替代方案**，用以完全跳过权限——你在减少提示弹窗的同时，仍保留一份可审计的允许清单。**支持完整的通配符语法**——试试 `"Bash(bun run *)"` 或 `"Edit(/docs/**)"`。

Claude Code's permission system layers prompt-injection detection, static analysis, sandboxing, and human oversight. A small set of safe commands is pre-approved out of the box; everything you add via `/permissions` is additive to that baseline.

Claude Code 的权限系统叠加了提示注入检测、静态分析、沙箱和人工监督。开箱即用即预先批准了一小批安全命令；你通过 `/permissions` 添加的一切都是在此基线之上的增量。

#### Auto mode

Auto mode lets Claude make permission decisions on your behalf. Classifiers evaluate each action before it runs — safe operations get auto-approved, risky ones still get flagged. Enable it with `claude --enable-auto-mode`; once enabled, **Shift+Tab** cycles `default → acceptEdits → plan → auto` during a session. Without that flag, the cycle is `default → acceptEdits → plan`.

#### 自动模式

自动模式让 Claude 代表你做出权限决策。分类器会在每个动作执行前评估它——安全操作自动批准，有风险的操作仍会被标记。用 `claude --enable-auto-mode` 启用；启用后，会话中 **Shift+Tab** 的循环为 `default → acceptEdits → plan → auto`。未加该参数时，循环为 `default → acceptEdits → plan`。

#### Sandboxing

Run `/sandbox` to opt into Claude Code's open-source sandbox runtime. It runs on your machine and supports both **file and network isolation**, improving safety while reducing permission prompts. Three modes are available:

#### 沙箱

运行 `/sandbox` 以启用 Claude Code 的开源沙箱运行时。它在你的机器上运行，同时支持**文件隔离和网络隔离**，在提升安全性的同时减少权限提示。提供三种模式：

-   Sandbox BashTool, with auto-allow

-   沙箱化 BashTool，自动允许

-   Sandbox BashTool, with regular permissions

-   沙箱化 BashTool，使用常规权限

-   No sandbox

-   不使用沙箱

#### Long-running tasks

For very long-running tasks, ensure Claude can work uninterrupted. Recommended approaches:

#### 长时间运行的任务

对于运行时间很长的任务，要确保 Claude 能不被打断地工作。推荐做法：

-   Prompt Claude to verify with a background agent when done.

-   让 Claude 在完成时用一个后台 agent 验证。

-   Use an agent `Stop` hook for deterministic checks (preferred for auditable workflows).

-   使用 agent 的 `Stop` hook 进行确定性检查（对可审计的工作流更受推荐）。

-   Use the "ralph-wiggum" community plugin.

-   使用 "ralph-wiggum" 社区插件。

For sandboxed environments, use `--permission-mode=dontAsk` or `--dangerously-skip-permissions` to avoid blocks.

在沙箱环境中，使用 `--permission-mode=dontAsk` 或 `--dangerously-skip-permissions` 以避免被阻断。

### Scheduled and recurring tasks

### 定时与循环任务

#### /loop for local recurring tasks

`/loop` schedules a recurring task locally for up to 3 days at a time. A few examples the Claude Code team runs:

#### 用于本地循环任务的 /loop

`/loop` 在本地调度循环任务，每次最多持续 3 天。以下是 Claude Code 团队运行的几个示例：

```
/loop 5m /babysit         # auto-address review, rebase, shepherd PRs
/loop 30m /slack-feedback # auto put up PRs for Slack feedback
/loop 1h /pr-pruner       # close out stale PRs
```

#### /schedule for Cloud Jobs

Unlike `/loop`, scheduled jobs run in the **cloud** — they keep working even when your laptop is closed.

#### 用于云端作业的 /schedule

与 `/loop` 不同，定时作业运行在**云端**——即使你的笔记本合上，它们也会继续工作。

```
> /schedule a daily job that looks at all PRs shipped since
  yesterday and updates our docs based on the changes. Use
  the Slack MCP to message #docs-update with the changes
```

**Note:** Experiment with turning your most common workflows into a skill + a loop. It's powerful.

**注意：** 试着把你最常用的工作流变成一个 skill + 一个 loop，效果会非常强大。

### Mobile and remote control

### 移动端与远程控制

#### Work from your phone

Claude Code has a **mobile app**—download the Claude app for iOS/Android and tap the Code tab. An **iMessage plugin** is also available (`/plugin install imessage@claude-plugins-official`) to send tasks from any Apple device.

#### 在手机上工作

Claude Code 有**移动应用**——下载 iOS/Android 版 Claude 应用并点击 Code 标签页。还提供了一个 **iMessage 插件**（`/plugin install imessage@claude-plugins-official`），可从任何 Apple 设备发送任务。

#### Teleport sessions between devices

Move sessions back and forth between mobile, web, desktop, and terminal:

#### 在设备之间 teleport 会话

在移动端、网页端、桌面端和终端之间来回迁移会话：

-   `claude --teleport` (or `/teleport` from inside a session) continues a cloud session on your machine.

-   `claude --teleport`（或在会话内使用 `/teleport`）在你的机器上继续一个云端会话。

-   `/remote-control` lets you control a local session from your phone or the web.

-   `/remote-control` 让你从手机或网页端控制一个本地会话。

-   `claude remote-control` lets you spawn a new local session from the mobile app. *Availability: Pro, Max, Team, and Enterprise plans on CLI v2.1.51+.*

-   `claude remote-control` 让你从移动应用派生一个新的本地会话。*可用范围：CLI v2.1.51+ 上的 Pro、Max、Team 和 Enterprise 套餐。*

You can also enable **"Enable Remote Control for all sessions"** in `/config`.

你也可以在 `/config` 中启用 **"Enable Remote Control for all sessions"**。

#### Claude Cowork Dispatch

Dispatch is a secure remote control for the Claude Desktop app. It can use your MCPs, browser, and computer with your permission—useful for catching up on Slack and emails, managing files, and doing things on your laptop when you're away from it.

#### Claude Cowork Dispatch

Dispatch 是 Claude 桌面应用的一种安全远程控制。在获得你许可的前提下，它可以使用你的 MCP、浏览器和计算机——适合在离开笔记本时处理 Slack 和邮件、管理文件，以及在你的笔记本上执行操作。

### Tool integrations (MCP)

Connect Claude to your existing tools so it can search Slack, run BigQuery, grab Sentry logs, and more. Add MCP servers via claude mcp add or the "mcpServers" block in settings.json — see **[code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp)** for configuration.

### 工具集成 (MCP)

将 Claude 连接到你现有的工具，让它能搜索 Slack、运行 BigQuery、抓取 Sentry 日志等。通过 claude mcp add 或 settings.json 中的 "mcpServers" 块来添加 MCP 服务器——配置详见 **[code.claude.com/docs/en/mcp](https://code.claude.com/docs/en/mcp)**。

#### Data and analytics

Ask Claude to use the `bq` CLI to pull and analyze metrics on the fly—keep a BigQuery skill checked into your codebase. The Claude Code team's take: "Personally, I haven't written a line of SQL in 6+ months." This works for any database that has a CLI, MCP, or API.

#### 数据与分析

让 Claude 用 `bq` CLI 实时拉取并分析指标——在代码库中维护一个 BigQuery skill 并提交到 git。Claude Code 团队的说法："就我个人而言，我已经 6 个多月没写过一行 SQL 了。"这对任何具备 CLI、MCP 或 API 的数据库都适用。

#### Bug fixing

Enable the Slack MCP, paste a bug thread into Claude, and just say **"fix"**—zero context switching. Or say **"go fix the failing CI tests"** without micromanaging how. Point Claude at **docker logs** to troubleshoot distributed systems—it's surprisingly capable at this.

#### bug 修复

启用 Slack MCP，把一个 bug 讨论串粘贴给 Claude，然后只说 **"fix"**——零上下文切换。或者直接说 **"去把失败的 CI 测试修好"**，而不去微观管理怎么修。把 Claude 指向 **docker logs** 来排查分布式系统问题——它在这方面的能力出人意料地强。

#### Plugins

Plugins bundle LSPs (available for every major language), MCPs, skills, agents, and custom hooks. Install from the official Anthropic plugin marketplace, or stand up an internal marketplace for your organization—then check the marketplace reference into `settings.json` so it's auto-added for every developer. Run `/plugin` to get started.

#### 插件

插件打包了 LSP（适用于所有主流语言）、MCP、skill、agent 和自定义 hook。可从官方 Anthropic 插件市场安装，或为你的组织搭建一个内部市场——然后把市场引用提交到 `settings.json`，这样每位开发者都会自动获得。运行 `/plugin` 即可上手。

### Customizing your environment

### 自定义你的环境

#### Terminal setup

Run `/config` to set light/dark mode and /terminal-setup to enable **Shift+Enter** for newlines in IDE terminals, Warp, or Alacritty (Apple Terminal is not supported by `/terminal-setup`). For Vim keybindings, open `/config` → Editor mode. The team recommends **Ghostty** for synchronized rendering and 24-bit color.

#### 终端设置

运行 `/config` 设置亮色/暗色模式，运行 /terminal-setup 在 IDE 终端、Warp 或 Alacritty 中启用 **Shift+Enter** 换行（`/terminal-setup` 不支持 Apple Terminal）。如需 Vim 键位绑定，打开 `/config` → Editor mode。团队推荐使用 **Ghostty** 以获得同步渲染和 24 位色彩。

#### Status line, color, and keybindings

-   `/statusline` generates a custom status line based on your `.bashrc`/`.zshrc`—show model, directory, remaining context, cost, or anything else.

#### 状态栏、颜色与键位绑定

-   `/statusline` 根据你的 `.bashrc`/`.zshrc` 生成自定义状态栏——显示模型、目录、剩余上下文、花费或其他任何信息。

-   `/color` changes the prompt input color—useful when you have 3–5 sessions open and need to tell them apart at a glance.

-   `/color` 改变 prompt 输入颜色——当你同时开了 3–5 个会话、需要一眼区分它们时非常有用。

-   `/keybindings` remaps any key. Settings live-reload and are stored in `~/.claude/keybindings.json`.

-   `/keybindings` 重新映射任意按键。设置会热重载，并存储在 `~/.claude/keybindings.json` 中。

#### Voice input

Voice mode is available to all users, including Claude Code Desktop and Cowork. Most of the Claude Code team's coding is done by speaking—you speak roughly 3× faster than you type, and your prompts get more detailed as a result.

#### 语音输入

语音模式对所有用户开放，包括 Claude Code 桌面版和 Cowork。Claude Code 团队大部分的编码工作都是通过说话完成的——你说话的速度大约是打字的 3 倍，因此你的 prompt 也会更详细。

-   **CLI:** run `/voice` then hold the space bar

-   **CLI：** 运行 `/voice` 然后按住空格键

-   **Desktop:** press the voice button (microphone icon)

-   **桌面端：** 按下语音按钮（麦克风图标）

-   **iOS:** enable dictation in your system settings

-   **iOS：** 在系统设置中启用听写

-   **macOS native:** hit fn×2 for system dictation in any terminal

-   **macOS 原生：** 在任意终端中连按两次 fn 键启动系统听写

#### Web sessions

Beyond the terminal, run additional sessions on [claude.ai/code](https://claude.ai/code). Use the `&` command to background a session, or the `--teleport` flag to switch contexts between local and web.

#### 网页会话

除了终端，你还可以在 [claude.ai/code](https://claude.ai/code) 上运行额外的会话。使用 `&` 命令把会话放到后台，或用 `--teleport` 参数在本地与网页之间切换上下文。

#### Output styles

Run `/config` and set an output style. **Explanatory** has Claude explain frameworks and patterns as it works (great for new codebases). **Learning** has Claude coach you through changes. You can also create **custom** styles to adjust Claude's voice.

#### 输出样式

运行 `/config` 并设置一个输出样式。**Explanatory** 让 Claude 在工作时解释框架与模式（非常适合新代码库）。**Learning** 让 Claude 像教练一样引导你完成变更。你也可以创建**自定义**样式来调整 Claude 的语气。

#### Spinner verbs

It's the little things that make Claude Code feel personal. Ask Claude to customize your spinner verbs to add to or replace the default list. Check the `settings.json` into source control to share verbs with your team.

#### Spinner 动词

正是这些细节让 Claude Code 显得有个性。请 Claude 自定义你的 spinner 动词，以添加或替换默认列表。把 `settings.json` 提交到版本控制，即可与团队共享这些动词。

#### Customize everything

Claude Code is built to work great out of the box, but when you do customize, **check `settings.json` into git** so your team benefits too. Configuration is supported per-codebase, per-subfolder, per-user, or via enterprise-wide policies.

#### 自定义一切

Claude Code 开箱即用就很好用，但当你真正去自定义时，**请把 `settings.json` 提交到 git**，让团队也受益。配置支持按代码库、按子文件夹、按用户，或通过企业级策略进行。

**By the numbers:** dozens of settings and environment variables—see the **[settings reference](https://code.claude.com/docs/en/settings)**. Use the `"env"` field in `settings.json` to avoid wrapper scripts.

**数据概览：** 数十个设置项与环境变量——参见 **[settings 参考](https://code.claude.com/docs/en/settings)**。使用 `settings.json` 中的 `"env"` 字段即可避免使用包装脚本。

### SDK and multi-repo work

### SDK 与多仓库工作

#### --bare for Faster SDK Startup

By default, `claude -p` (and the TypeScript/Python SDKs) searches for local `CLAUDE.md` files, settings, and MCPs. For non-interactive usage, you usually want to specify these explicitly via `--system-prompt`, `--mcp-config`, `--settings`, etc. Add `--bare` for roughly 10× faster startup:

#### 用于加速 SDK 启动的 --bare

默认情况下，`claude -p`（以及 TypeScript/Python SDK）会搜索本地的 `CLAUDE.md` 文件、设置和 MCP。对于非交互式使用，你通常希望通过 `--system-prompt`、`--mcp-config`、`--settings` 等显式指定这些内容。加上 `--bare` 可获得约 10 倍的启动加速：

```
claude -p "summarize this codebase" \
    --output-format=stream-json \
    --verbose \
    --bare
```

**Note:** This was a design oversight when the SDK was first built. In a future version, the default will flip to `--bare`. For now, opt in with the flag.

**注意：** 这是 SDK 最初构建时的设计疏忽。在未来的版本中，默认将切换为 `--bare`。目前请通过该参数手动启用。

#### --add-dir for multi-repo work

When working across repositories, use `--add-dir` (or `/add-dir`) to give Claude access and permissions to additional folders. Or add `"additionalDirectories"` to your team's `settings.json` to always include them.

#### 用于多仓库工作的 --add-dir

在跨仓库工作时，使用 `--add-dir`（或 `/add-dir`）为 Claude 授予额外文件夹的访问权和权限。或者在团队的 `settings.json` 中添加 `"additionalDirectories"`，以便始终包含这些目录。

#### Forking a session

To branch off an existing session, run `/branch` from inside it, or `claude --resume <session-id> --fork-session` from the CLI.

#### fork 一个会话

要从现有会话分叉出来，可在会话内运行 `/branch`，或在 CLI 中运行 `claude --resume <session-id> --fork-session`。

#### Setup scripts for cloud environments

In Claude Code on web and desktop, add a **setup script** that runs before each new cloud session—install dependencies, configure settings, set environment variables. The script is skipped on resume.

#### 用于云环境的 setup 脚本

在网页端和桌面端的 Claude Code 中，可以添加一个**setup 脚本**，在每次新建云端会话前运行——安装依赖、配置设置、设定环境变量。该脚本在 resume（恢复）时会被跳过。

### Appendix: Quick reference

**Area** | **Key commands**

### 附录：快速参考

**领域** | **关键命令**

Parallel work | `claude --worktree`, `/batch`, `isolation: worktree`

并行工作 | `claude --worktree`、`/batch`、`isolation: worktree`

Planning | Shift+Tab, `/effort max`, `claude --name`

规划 | Shift+Tab、`/effort max`、`claude --name`

Memory | `CLAUDE.md`, `/memory`, `/dream`, `@claude` in PRs

记忆 | `CLAUDE.md`、`/memory`、`/dream`、PR 中的 `@claude`

Verification | Chrome extension, `/simplify`, Desktop app

验证 | Chrome 扩展、`/simplify`、桌面应用

Automation | `.claude/skills/`, `.claude/agents/`, `--agent`

自动化 | `.claude/skills/`、`.claude/agents/`、`--agent`

Hooks | `PostToolUse`, `Stop`, `PostCompact`, `PermissionRequest`

hook | `PostToolUse`、`Stop`、`PostCompact`、`PermissionRequest`

Permissions | `/permissions`, auto mode, `/sandbox`

权限 | `/permissions`、自动模式、`/sandbox`

Scheduling | `/loop`, `/schedule`

调度 | `/loop`、`/schedule`

Remote | `--teleport`, `/remote-control`, mobile app, iMessage

远程 | `--teleport`、`/remote-control`、移动应用、iMessage

Customization | `/statusline`, `/color`, `/voice`, `/keybindings`, `/config`

自定义 | `/statusline`、`/color`、`/voice`、`/keybindings`、`/config`

SDK & multi-repo | `--bare`, `--add-dir`, `/branch`

SDK 与多仓库 | `--bare`、`--add-dir`、`/branch`

### Appendix: Related articles

**Resource** | **Link**

### 附录：相关文章

**资源** | **链接**

Hooks reference | **[code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks)**

hook 参考 | **[code.claude.com/docs/en/hooks](https://code.claude.com/docs/en/hooks)**

Subagents and custom agents | **[code.claude.com/docs/en/sub-agents](https://code.claude.com/docs/en/sub-agents)**

subagent 与自定义 agent | **[code.claude.com/docs/en/sub-agents](https://code.claude.com/docs/en/sub-agents)**

Scheduled tasks | **[code.claude.com/docs/en/scheduled-tasks](https://code.claude.com/docs/en/scheduled-tasks)**

定时任务 | **[code.claude.com/docs/en/scheduled-tasks](https://code.claude.com/docs/en/scheduled-tasks)**

Chrome extension | **[code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome)**

Chrome 扩展 | **[code.claude.com/docs/en/chrome](https://code.claude.com/docs/en/chrome)**

Claude Code ships frequently. Verify version-specific details against **[code.claude.com/docs](https://code.claude.com/docs)** before distributing internally.

Claude Code 发布频繁。在内部分发前，请对照 **[code.claude.com/docs](https://code.claude.com/docs)** 核实与版本相关的细节。


---

## Claude Code - Extend Claude with skills（用 skill 扩展 Claude）

原文链接：https://code.claude.com/docs/en/skills

### Extend Claude with skills

> Create, manage, and share skills to extend Claude's capabilities in Claude Code. Includes custom commands and bundled skills.

> 创建、管理并分享 skill（技能），以扩展 Claude Code 中 Claude 的能力。包含自定义命令与内置技能。

Skills extend what Claude can do. Create a `SKILL.md` file with instructions, and Claude adds it to its toolkit. Claude uses skills when relevant, or you can invoke one directly with `/skill-name`.

Skill 扩展了 Claude 的能力。创建一个带指令的 `SKILL.md` 文件，Claude 就会把它加入自己的工具箱。Claude 会在相关时使用 skill，你也可以用 `/skill-name` 直接调用某个 skill。

Create a skill when you keep pasting the same instructions, checklist, or multi-step procedure into chat, or when a section of CLAUDE.md has grown into a procedure rather than a fact. Unlike CLAUDE.md content, a skill's body loads only when it's used, so long reference material costs almost nothing until you need it.

当你反复把同样的指令、清单或多步骤流程粘贴到对话里时，或者当 CLAUDE.md 中的某一段已经从"事实说明"演变成"操作流程"时，就该创建一个 skill 了。与 CLAUDE.md 内容不同，skill 的正文只有在被使用时才会加载，因此冗长的参考资料在用不到时几乎零开销。

<Note>
For built-in commands like `/help` and `/compact`, and bundled skills like `/debug` and `/code-review`, see the [commands reference](/en/commands).

**Custom commands have been merged into skills.** A file at `.claude/commands/deploy.md` and a skill at `.claude/skills/deploy/SKILL.md` both create `/deploy` and work the same way. Your existing `.claude/commands/` files keep working. Skills add optional features: a directory for supporting files, frontmatter to [control whether you or Claude invokes them](#control-who-invokes-a-skill), and the ability for Claude to load them automatically when relevant.
</Note>

<Note>
对于 `/help` 和 `/compact` 这类内置命令，以及 `/debug` 和 `/code-review` 这类内置 skill，请参见[命令参考](/en/commands)。

**自定义命令已并入 skill。** `.claude/commands/deploy.md` 文件与 `.claude/skills/deploy/SKILL.md` skill 都会创建 `/deploy`，二者效果一致。你现有的 `.claude/commands/` 文件仍然可用。skill 增加了若干可选能力：一个用于存放辅助文件的目录、用于[控制由你还是 Claude 来调用](#control-who-invokes-a-skill)的 frontmatter，以及让 Claude 在相关时自动加载的能力。
</Note>

Claude Code skills follow the [Agent Skills](https://agentskills.io) open standard, which works across multiple AI tools. Claude Code extends the standard with additional features like [invocation control](#control-who-invokes-a-skill), [subagent execution](#run-skills-in-a-subagent), and [dynamic context injection](#inject-dynamic-context).

Claude Code 的 skill 遵循 [Agent Skills](https://agentskills.io) 开放标准，该标准可在多种 AI 工具间通用。Claude Code 在此标准之上扩展了额外能力，如[调用控制](#control-who-invokes-a-skill)、[subagent（子代理）执行](#run-skills-in-a-subagent)以及[动态上下文注入](#inject-dynamic-context)。

### Bundled skills

Claude Code includes a set of bundled skills that are available in every session unless disabled with the [`disableBundledSkills`](/en/settings#available-settings) setting, including `/code-review`, `/batch`, `/debug`, `/loop`, and `/claude-api`. Unlike most built-in commands, which execute fixed logic directly, bundled skills are prompt-based: they give Claude detailed instructions and let it orchestrate the work using its tools. You invoke them the same way as any other skill, by typing `/` followed by the skill name.

Claude Code 内置了一组 skill，除非通过 [`disableBundledSkills`](/en/settings#available-settings) 设置禁用，否则在每次会话中都可用，包括 `/code-review`、`/batch`、`/debug`、`/loop` 和 `/claude-api`。与大多数直接执行固定逻辑的内置命令不同，内置 skill 是基于 prompt（提示）的：它们给 Claude 提供详细指令，让其用自身工具来编排工作。调用方式与其它 skill 一致，输入 `/` 后跟 skill 名称即可。

Bundled skills are listed alongside built-in commands in the [commands reference](/en/commands), marked **Skill** in the Purpose column.

内置 skill 与内置命令一起列在[命令参考](/en/commands)中，在 Purpose（用途）列中标注为 **Skill**。

### Run and verify your app

Three bundled skills work together to launch your app and confirm changes against the running app instead of just tests:

三个内置 skill 协同工作，用于启动你的应用，并针对正在运行的应用确认改动效果，而不仅仅依赖测试：

| Skill                  | Purpose                                                                                                           |
| :--------------------- | :---------------------------------------------------------------------------------------------------------------- |
| `/run`                 | Launch and drive your app to see a change working                                                                 |
| `/verify`              | Build and run your app to confirm a code change does what it should, without falling back to tests or type checks |
| `/run-skill-generator` | Teach `/run` and `/verify` how to build and launch your project                                                   |

| Skill                  | Purpose（用途）                                                                                                           |
| :--------------------- | :---------------------------------------------------------------------------------------------------------------- |
| `/run`                 | 启动并驱动你的应用，查看改动是否生效                                                                 |
| `/verify`              | 构建并运行你的应用，确认代码改动达到了预期效果，不退回到测试或类型检查 |
| `/run-skill-generator` | 教 `/run` 和 `/verify` 如何构建并启动你的项目                                                  |

All three skills require Claude Code v2.1.145 or later.

以上三个 skill 均要求 Claude Code v2.1.145 或更高版本。

`/run` and `/verify` work without setup. They infer the launch from your project type (CLI, server, TUI, browser-driven) and from what's in your README, `package.json`, or `Makefile`. That inference gets unreliable for projects that need anything beyond a standard launch: a database, an env file, a graphical session, a multi-step build.

`/run` 和 `/verify` 无需配置即可使用。它们会根据项目类型（CLI、server、TUI、浏览器驱动）以及 README、`package.json` 或 `Makefile` 中的内容来推断启动方式。对于需要标准启动之外的依赖——数据库、env 文件、图形会话、多步骤构建——的项目，这种推断就不够可靠。

`/run-skill-generator` records the recipe instead. It gets your app running from a clean environment, captures what worked (the install commands, the env vars, the launch script), and commits it as a per-project skill at `.claude/skills/run-<name>/`. After that, `/run`, `/verify`, and any other agent in the repo follow the recorded recipe instead of rediscovering it. Run `/run-skill-generator` once per project, and again if the build or launch process changes.

`/run-skill-generator` 则改为记录"配方"。它在干净环境中把你的应用跑起来，捕获生效的内容（安装命令、环境变量、启动脚本），并提交为项目级 skill，存于 `.claude/skills/run-<name>/`。此后，`/run`、`/verify` 以及仓库中的任何 agent 都会遵循已记录的配方，而不必重新摸索。每个项目运行一次 `/run-skill-generator`；若构建或启动流程发生变化，再运行一次。

### Getting started

#### Create your first skill

This example creates a skill that summarizes the uncommitted changes in your git repository and flags anything risky. It pulls the live diff into the prompt before Claude reads it, so the response is grounded in your actual working tree rather than what Claude can guess from open files. Claude loads the skill automatically when you ask about your changes, or you can invoke it directly with `/summarize-changes`.

本示例创建一个 skill，用于总结 git 仓库中未提交的改动并标记有风险之处。它在 Claude 读取之前把实时 diff 拉进 prompt，因此回复基于你的实际工作区，而非 Claude 从已打开文件中的猜测。当你询问自己的改动时，Claude 会自动加载该 skill，你也可以用 `/summarize-changes` 直接调用。

**Step 1: Create the skill directory**

Create a directory for the skill in your personal skills folder. Personal skills are available across all your projects.

**第 1 步：创建 skill 目录**

在个人 skill 文件夹中为该 skill 创建一个目录。个人 skill 在你所有项目中都可用。

```bash
mkdir -p ~/.claude/skills/summarize-changes
```

**Step 2: Write SKILL.md**

Every skill needs a `SKILL.md` file with two parts: YAML frontmatter between `---` markers that tells Claude when to use the skill, and markdown content with the instructions Claude follows when the skill runs. The directory name becomes the command you type, and the `description` helps Claude decide when to load the skill automatically.

**第 2 步：编写 SKILL.md**

每个 skill 都需要一个 `SKILL.md` 文件，包含两部分：`---` 标记之间的 YAML frontmatter，用于告诉 Claude 何时使用该 skill；以及 markdown 正文，即 skill 运行时 Claude 遵循的指令。目录名会成为你输入的命令，`description` 则帮助 Claude 决定何时自动加载该 skill。

Save this to `~/.claude/skills/summarize-changes/SKILL.md`:

将其保存到 `~/.claude/skills/summarize-changes/SKILL.md`：

```yaml
---
description: Summarizes uncommitted changes and flags anything risky. Use when the user asks what changed, wants a commit message, or asks to review their diff.
---

## Current changes

!`git diff HEAD`

## Instructions

Summarize the changes above in two or three bullet points, then list any risks you notice such as missing error handling, hardcoded values, or tests that need updating. If the diff is empty, say there are no uncommitted changes.
```

The `` !`git diff HEAD` `` line uses [dynamic context injection](#inject-dynamic-context): Claude Code runs the command and replaces the line with its output before Claude sees the skill content, so the instructions arrive with the current diff already inlined.

`` !`git diff HEAD` `` 这一行使用了[动态上下文注入](#inject-dynamic-context)：Claude Code 会运行该命令，并在 Claude 看到 skill 内容之前把该行替换为命令输出，因此指令送达时已内联了当前的 diff。

**Step 3: Test the skill**

Open a git project, make a small edit to any file, and start Claude Code by running `claude`. You can test the skill two ways.

**第 3 步：测试 skill**

打开一个 git 项目，对任意文件做一处小改动，然后运行 `claude` 启动 Claude Code。可以通过两种方式测试该 skill。

**Let Claude invoke it automatically** by asking something that matches the description:

**让 Claude 自动调用**，即提出一个与 description 匹配的问题：

```text
What did I change?
```

**Or invoke it directly** with the skill name:

**或用 skill 名称直接调用**：

```text
/summarize-changes
```

Either way, Claude should respond with a short summary of your edit and a list of risks.

无论哪种方式，Claude 都应回复一段关于你改动的简短总结，并附上风险列表。

### Where skills live

Where you store a skill determines who can use it:

skill 存放的位置决定了谁能使用它：

| Location   | Path                                                | Applies to                     |
| :--------- | :-------------------------------------------------- | :----------------------------- |
| Enterprise | See [managed settings](/en/settings#settings-files) | All users in your organization |
| Personal   | `~/.claude/skills/<skill-name>/SKILL.md`            | All your projects              |
| Project    | `.claude/skills/<skill-name>/SKILL.md`              | This project only              |
| Plugin     | `<plugin>/skills/<skill-name>/SKILL.md`             | Where plugin is enabled        |

| 位置   | 路径                                                | 适用范围                     |
| :--------- | :-------------------------------------------------- | :----------------------------- |
| Enterprise（企业级） | 参见 [managed settings](/en/settings#settings-files) | 组织内所有用户 |
| Personal（个人级）   | `~/.claude/skills/<skill-name>/SKILL.md`            | 你的所有项目              |
| Project（项目级）    | `.claude/skills/<skill-name>/SKILL.md`              | 仅本项目              |
| Plugin（插件级）     | `<plugin>/skills/<skill-name>/SKILL.md`             | 启用该 plugin 的地方        |

When skills share the same name across levels, enterprise overrides personal, and personal overrides project. A skill at any of these levels also overrides a bundled skill with the same name. For example, a `code-review` skill in your project's `.claude/skills/` replaces the bundled `/code-review`. Plugin skills use a `plugin-name:skill-name` namespace, so they cannot conflict with other levels. If you have files in `.claude/commands/`, those work the same way, but if a skill and a command share the same name, the skill takes precedence.

当不同级别的 skill 同名时，enterprise 覆盖 personal，personal 覆盖 project。任一级别的 skill 也会覆盖同名的内置 skill。例如，项目 `.claude/skills/` 中的 `code-review` skill 会替换内置的 `/code-review`。Plugin skill 使用 `plugin-name:skill-name` 命名空间，因此不会与其他级别冲突。如果你在 `.claude/commands/` 中有文件，其行为相同；但当 skill 与 command 同名时，skill 优先。

Skills also load from nested `.claude/skills/` directories below your working directory. When Claude reads or edits a file in a subdirectory, skills from that subdirectory's `.claude/skills/` become available. This lets a monorepo package provide its own skills that apply when working on that package, even if the session started at the repo root.

skill 还会从工作目录下嵌套的 `.claude/skills/` 目录加载。当 Claude 读取或编辑子目录中的文件时，该子目录 `.claude/skills/` 中的 skill 就变得可用。这让 monorepo 中的某个 package 可以提供自己的 skill，在处理该 package 时生效，即便会话是在仓库根目录启动的。

If a nested skill shares a name with another skill, both stay available. For example, with a `deploy` skill at the project root and another in `apps/web/.claude/skills/`:

若嵌套 skill 与其它 skill 同名，两者都保持可用。例如，项目根有一个 `deploy` skill，`apps/web/.claude/skills/` 中又有一个：

* The nested one appears under a directory-qualified name, `apps/web:deploy`.
* Its description says which directory it applies to.
* Claude picks the variant that matches the files it is working on.

* 嵌套的那个以带目录前缀的名称出现，即 `apps/web:deploy`。
* 其 description 会说明它适用于哪个目录。
* Claude 会根据正在处理的文件选择匹配的版本。

Typing `/deploy` runs the project-root skill. Type the qualified name `/apps/web:deploy` to run the nested variant explicitly.

输入 `/deploy` 会运行项目根的 skill。输入带前缀的名称 `/apps/web:deploy` 可显式运行嵌套版本。

A `<skill-name>` entry in the enterprise, personal, or project locations can be a symlink to a directory elsewhere on disk. Claude Code follows the symlink and reads `SKILL.md` from the target directory, and if the same target is reachable from more than one location, Claude Code loads the skill once. Plugin skills handle symlinks differently; see [Share files within a marketplace with symlinks](/en/plugins-reference#share-files-within-a-marketplace-with-symlinks).

enterprise、personal 或 project 位置中的 `<skill-name>` 条目可以是指向磁盘上其它目录的 symlink（符号链接）。Claude Code 会跟随该 symlink 从目标目录读取 `SKILL.md`；若同一目标可从多个位置到达，Claude Code 只加载一次。Plugin skill 对 symlink 的处理方式不同，参见[在 marketplace 内用 symlink 共享文件](/en/plugins-reference#share-files-within-a-marketplace-with-symlinks)。

<Note>
Add a `.claude-plugin/plugin.json` to a skill folder and it loads as a [plugin](/en/plugins-reference#skills-directory-plugins) named `<name>@skills-dir`, so it can bundle agents, hooks, and MCP servers. In a project's `.claude/skills/`, this requires accepting the workspace trust dialog first.
</Note>

<Note>
在 skill 文件夹中添加 `.claude-plugin/plugin.json`，它就会以名为 `<name>@skills-dir` 的 [plugin（插件）](/en/plugins-reference#skills-directory-plugins) 加载，从而可以打包 agent、hook（钩子）和 MCP server。在项目的 `.claude/skills/` 中，这需要先接受 workspace 信任对话框。
</Note>

#### Live change detection

Claude Code watches skill directories for file changes. Adding, editing, or removing a skill under `~/.claude/skills/`, the project `.claude/skills/`, or a `.claude/skills/` inside an `--add-dir` directory takes effect within the current session without restarting. Creating a top-level skills directory that did not exist when the session started requires restarting Claude Code so the new directory can be watched.

#### 实时变更检测

Claude Code 会监视 skill 目录的文件变动。在 `~/.claude/skills/`、项目 `.claude/skills/`、或 `--add-dir` 目录内的 `.claude/skills/` 下新增、编辑或删除 skill，都会在当前会话中立即生效，无需重启。若要新建一个会话启动时并不存在的顶层 skills 目录，则需要重启 Claude Code，以便对新目录进行监视。

<Note>
Live change detection covers `SKILL.md` text only. For a skill folder that is also a [plugin](/en/plugins-reference#skills-directory-plugins), changes to `hooks/`, `.mcp.json`, `agents/`, and `output-styles/` need `/reload-plugins` to take effect.
</Note>

<Note>
实时变更检测仅覆盖 `SKILL.md` 文本。对于同时也是 [plugin](/en/plugins-reference#skills-directory-plugins) 的 skill 文件夹，对 `hooks/`、`.mcp.json`、`agents/` 和 `output-styles/` 的改动需要执行 `/reload-plugins` 才能生效。
</Note>

#### Automatic discovery from parent and nested directories

Project skills load from `.claude/skills/` in your starting directory and in every parent directory up to the repository root, so starting Claude in a subdirectory still picks up skills defined at the root. When you work with files in subdirectories below your starting directory, Claude Code also discovers skills from nested `.claude/skills/` directories on demand. For example, if you're editing a file in `packages/frontend/`, Claude Code also looks for skills in `packages/frontend/.claude/skills/`. This supports monorepo setups where packages have their own skills.

#### 从父目录与嵌套目录自动发现

项目 skill 会从你起始目录以及向上直至仓库根的每个父目录中的 `.claude/skills/` 加载，因此在子目录中启动 Claude 仍能获取根目录定义的 skill。当你在起始目录之下的子目录中处理文件时，Claude Code 还会按需从嵌套的 `.claude/skills/` 目录发现 skill。例如，若你正在编辑 `packages/frontend/` 中的文件，Claude Code 也会查找 `packages/frontend/.claude/skills/` 中的 skill。这支持 monorepo 中各 package 拥有自己 skill 的场景。

Each skill is a directory with `SKILL.md` as the entrypoint:

每个 skill 都是一个以 `SKILL.md` 为入口的目录：

```text
my-skill/
├── SKILL.md           # Main instructions (required)
├── template.md        # Template for Claude to fill in
├── examples/
│   └── sample.md      # Example output showing expected format
└── scripts/
    └── validate.sh    # Script Claude can execute
```

The `SKILL.md` contains the main instructions and is required. Other files are optional and let you build more powerful skills: templates for Claude to fill in, example outputs showing the expected format, scripts Claude can execute, or detailed reference documentation. Reference these files from your `SKILL.md` so Claude knows what they contain and when to load them. See [Add supporting files](#add-supporting-files) for more details.

`SKILL.md` 包含主要指令，是必需的。其它文件可选，用于构建更强大的 skill：供 Claude 填充的模板、展示预期格式的示例输出、Claude 可执行的脚本，或详细的参考文档。在 `SKILL.md` 中引用这些文件，让 Claude 知道它们的内容以及何时加载。详见[添加辅助文件](#add-supporting-files)。

<Note>
Files in `.claude/commands/` still work and support the same [frontmatter](#frontmatter-reference). Skills are recommended since they support additional features like supporting files.
</Note>

<Note>
`.claude/commands/` 中的文件仍然可用，并支持相同的 [frontmatter](#frontmatter-reference)。推荐使用 skill，因为它支持辅助文件等额外能力。
</Note>

#### Skills from additional directories

The `--add-dir` flag and `/add-dir` command [grant file access](/en/permissions#additional-directories-grant-file-access-not-configuration) rather than configuration discovery, but skills are an exception: `.claude/skills/` within an added directory is loaded automatically. This exception applies only to `--add-dir` and `/add-dir`. The `permissions.additionalDirectories` setting in `settings.json` grants file access only and does not load skills. See [Live change detection](#live-change-detection) for how edits are picked up during a session.

#### 来自额外目录的 skill

`--add-dir` 标志和 `/add-dir` 命令[授予的是文件访问权限](/en/permissions#additional-directories-grant-file-access-not-configuration)，而非配置发现能力，但 skill 是个例外：被添加目录中的 `.claude/skills/` 会自动加载。此例外仅适用于 `--add-dir` 和 `/add-dir`。`settings.json` 中的 `permissions.additionalDirectories` 设置仅授予文件访问权限，不加载 skill。关于会话中如何拾取编辑，参见[实时变更检测](#live-change-detection)。

Other `.claude/` configuration such as commands and output styles is not loaded from additional directories. See the [exceptions table](/en/permissions#additional-directories-grant-file-access-not-configuration) for the complete list of what is and isn't loaded, and the recommended ways to share configuration across projects.

其它 `.claude/` 配置（如命令和输出样式）不会从额外目录加载。完整列表（加载什么、不加载什么）及跨项目共享配置的推荐方式，参见[例外表](/en/permissions#additional-directories-grant-file-access-not-configuration)。

<Note>
CLAUDE.md files from `--add-dir` directories are not loaded by default. To load them, set `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1`. See [Load from additional directories](/en/memory#load-from-additional-directories).
</Note>

<Note>
`--add-dir` 目录中的 CLAUDE.md 文件默认不加载。若要加载，请设置 `CLAUDE_CODE_ADDITIONAL_DIRECTORIES_CLAUDE_MD=1`。参见[从额外目录加载](/en/memory#load-from-additional-directories)。
</Note>

### Configure skills

Skills are configured through YAML frontmatter at the top of `SKILL.md` and the markdown content that follows.

### 配置 skill

skill 通过 `SKILL.md` 顶部的 YAML frontmatter 和随后的 markdown 内容进行配置。

#### Types of skill content

Skill files can contain any instructions, but thinking about how you want to invoke them helps guide what to include:

#### skill 内容的类型

skill 文件可包含任意指令，但想清楚你希望如何调用它们，有助于决定该写什么：

**Reference content** adds knowledge Claude applies to your current work. Conventions, patterns, style guides, domain knowledge. This content runs inline so Claude can use it alongside your conversation context.

**参考型内容**为 Claude 当前的工作添加知识：约定、模式、风格指南、领域知识。这类内容以内联方式运行，Claude 可将其与对话上下文一起使用。

```yaml
---
name: api-conventions
description: API design patterns for this codebase
---

When writing API endpoints:
- Use RESTful naming conventions
- Return consistent error formats
- Include request validation
```

**Task content** gives Claude step-by-step instructions for a specific action, like deployments, commits, or code generation. These are often actions you want to invoke directly with `/skill-name` rather than letting Claude decide when to run them. Add `disable-model-invocation: true` to prevent Claude from triggering it automatically.

**任务型内容**为 Claude 提供针对某个具体动作的分步指令，例如部署、提交或代码生成。这类通常是希望用 `/skill-name` 直接调用的动作，而非让 Claude 自行决定何时运行。添加 `disable-model-invocation: true` 可阻止 Claude 自动触发。

```yaml
---
name: deploy
description: Deploy the application to production
context: fork
disable-model-invocation: true
---

Deploy the application:
1. Run the test suite
2. Build the application
3. Push to the deployment target
```

Your `SKILL.md` can contain anything, but thinking through how you want the skill invoked (by you, by Claude, or both) and where you want it to run (inline or in a subagent) helps guide what to include. For complex skills, you can also [add supporting files](#add-supporting-files) to keep the main skill focused.

`SKILL.md` 可包含任何内容，但想清楚你希望由谁调用（你、Claude、或两者）以及在何处运行（内联还是 subagent），有助于决定该写什么。对于复杂的 skill，你还可以[添加辅助文件](#add-supporting-files)以保持主 skill 聚焦。

Keep the body itself concise. Once a skill loads, its content [stays in context across turns](#skill-content-lifecycle), so every line is a recurring token cost. State what to do rather than narrating how or why, and apply the same conciseness test you would for [CLAUDE.md content](/en/best-practices#write-an-effective-claude-md).

正文本身要保持简洁。skill 一旦加载，其内容会[在多轮对话中留在上下文里](#skill-content-lifecycle)，因此每一行都是反复出现的 token 成本。陈述"做什么"，而非叙述"怎么做"或"为什么"，并采用与 [CLAUDE.md 内容](/en/best-practices#write-an-effective-claude-md)相同的简洁标准。

#### Frontmatter reference

Beyond the markdown content, you can configure skill behavior using YAML frontmatter fields between `---` markers at the top of your `SKILL.md` file:

#### Frontmatter 参考

除 markdown 内容外，你还可以通过 `SKILL.md` 文件顶部 `---` 标记之间的 YAML frontmatter 字段来配置 skill 行为：

```yaml
---
name: my-skill
description: What this skill does
disable-model-invocation: true
allowed-tools: Read Grep
---

Your skill instructions here...
```

All fields are optional. Only `description` is recommended so Claude knows when to use the skill.

所有字段均为可选。仅推荐 `description`，以便 Claude 知道何时使用该 skill。

| Field                      | Required    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :------------------------- | :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                     | No          | Display name shown in skill listings. Defaults to the directory name. See [How a skill gets its command name](#how-a-skill-gets-its-command-name) for how this differs from the name you type to invoke the skill.                                                                                                                                                                                                                               |
| `description`              | Recommended | What the skill does and when to use it. Claude uses this to decide when to apply the skill. If omitted, uses the first paragraph of markdown content. Put the key use case first: the combined `description` and `when_to_use` text is truncated at 1,536 characters in the skill listing to reduce context usage.                                                                                                                               |
| `when_to_use`              | No          | Additional context for when Claude should invoke the skill, such as trigger phrases or example requests. Appended to `description` in the skill listing and counts toward the 1,536-character cap.                                                                                                                                                                                                                                               |
| `argument-hint`            | No          | Hint shown during autocomplete to indicate expected arguments. Example: `[issue-number]` or `[filename] [format]`.                                                                                                                                                                                                                                                                                                                               |
| `arguments`                | No          | Named positional arguments for [`$name` substitution](#available-string-substitutions) in the skill content. Accepts a space-separated string or a YAML list. Names map to argument positions in order.                                                                                                                                                                                                                                          |
| `disable-model-invocation` | No          | Set to `true` to prevent Claude from automatically loading this skill. Use for workflows you want to trigger manually with `/name`. Also prevents the skill from being [preloaded into subagents](/en/sub-agents#preload-skills-into-subagents). As of v2.1.196, also prevents the skill from running when a [scheduled task](/en/scheduled-tasks) fires with the skill as its prompt. Default: `false`.             |
| `user-invocable`           | No          | Set to `false` to hide from the `/` menu. Use for background knowledge users shouldn't invoke directly. Default: `true`.                                                                                                                                                                                                                                                                                                                         |
| `allowed-tools`            | No          | Tools Claude can use without asking permission when this skill is active. Accepts a space- or comma-separated string, or a YAML list.                                                                                                                                                                                                                                                                                                            |
| `disallowed-tools`         | No          | Tools removed from Claude's available pool while this skill is active. Use for autonomous skills that should never call certain tools, such as `AskUserQuestion` for a background loop. Accepts a space- or comma-separated string, or a YAML list. The restriction clears when you send your next message.                                                                                                                                      |
| `model`                    | No          | Model to use when this skill is active. The override applies for the rest of the current turn and is not saved to settings; the session model resumes on your next prompt. Accepts the same values as [`/model`](/en/model-config), or `inherit` to keep the active model. A value excluded by your organization's [`availableModels`](/en/model-config#restrict-model-selection) allowlist is not used and the session keeps its current model. |
| `effort`                   | No          | [Effort level](/en/model-config#adjust-effort-level) when this skill is active. Overrides the session effort level. Default: inherits from session. Options: `low`, `medium`, `high`, `xhigh`, `max`; available levels depend on the model.                                                                                                                                                                                                      |
| `context`                  | No          | Set to `fork` to run in a forked subagent context.                                                                                                                                                                                                                                                                                                                                                                                               |
| `agent`                    | No          | Which subagent type to use when `context: fork` is set.                                                                                                                                                                                                                                                                                                                                                                                          |
| `hooks`                    | No          | Hooks scoped to this skill's lifecycle. See [Hooks in skills and agents](/en/hooks#hooks-in-skills-and-agents) for configuration format.                                                                                                                                                                                                                                                                                                         |
| `paths`                    | No          | Glob patterns that limit when this skill is activated. Accepts a comma-separated string or a YAML list. When set, Claude loads the skill automatically only when working with files matching the patterns. Uses the same format as [path-specific rules](/en/memory#path-specific-rules).                                                                                                                                                        |
| `shell`                    | No          | Shell to use for `` !`command` `` and ` ```! ` blocks in this skill. Accepts `bash` (default) or `powershell`. Setting `powershell` runs inline shell commands via PowerShell on Windows. Requires `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`.                                                                                                                                                                                                          |

| 字段                      | 必填    | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| :------------------------- | :---------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                     | 否          | 在 skill 列表中显示的名称。默认为目录名。关于它与调用时输入名称的区别，参见 [skill 如何获得命令名](#how-a-skill-gets-its-command-name)。                                                                                                                                                                                                                               |
| `description`              | 推荐 | skill 的作用及使用时机。Claude 依据它决定何时应用该 skill。若省略，使用 markdown 内容的第一段。把关键用例放最前：skill 列表中 `description` 与 `when_to_use` 合计文本在 1,536 字符处截断，以降低上下文消耗。                                                                                                                               |
| `when_to_use`              | 否          | 关于 Claude 何时调用该 skill 的补充上下文，如触发短语或示例请求。在 skill 列表中追加到 `description`，并计入 1,536 字符上限。                                                                                                                                                                                                                                               |
| `argument-hint`            | 否          | 自动补全时显示的提示，表明预期参数。例如 `[issue-number]` 或 `[filename] [format]`。                                                                                                                                                                                                                                                                                                                               |
| `arguments`                | 否          | 用于 skill 内容中 [`$name` 替换](#available-string-substitutions)的具名位置参数。接受空格分隔字符串或 YAML 列表。名称按顺序映射到参数位置。                                                                                                                                                                                                                                          |
| `disable-model-invocation` | 否          | 设为 `true` 可阻止 Claude 自动加载该 skill。用于你希望用 `/name` 手动触发的工作流。还会阻止该 skill 被[预加载到 subagent](/en/sub-agents#preload-skills-into-subagents)。自 v2.1.196 起，当[定时任务](/en/scheduled-tasks)以该 skill 为 prompt 触发时也不会运行。默认：`false`。             |
| `user-invocable`           | 否          | 设为 `false` 可在 `/` 菜单中隐藏。用于不应被用户直接调用的背景知识。默认：`true`。                                                                                                                                                                                                                                                                                                                         |
| `allowed-tools`            | 否          | 该 skill 激活时 Claude 无需请求许可即可使用的工具。接受空格或逗号分隔字符串，或 YAML 列表。                                                                                                                                                                                                                                                                                                            |
| `disallowed-tools`         | 否          | 该 skill 激活时从 Claude 可用池中移除的工具。用于不应调用某些工具的自主型 skill，例如后台循环中的 `AskUserQuestion`。接受空格或逗号分隔字符串，或 YAML 列表。限制在你发送下一条消息时清除。                                                                                                                                      |
| `model`                    | 否          | 该 skill 激活时使用的模型。覆盖在当前轮次剩余部分生效，不写入设置；下一条 prompt 时恢复会话模型。接受与 [`/model`](/en/model-config) 相同的值，或 `inherit` 保持当前模型。被组织 [`availableModels`](/en/model-config#restrict-model-selection) 白名单排除的值不会被使用，会话保持当前模型。 |
| `effort`                   | 否          | 该 skill 激活时的 [effort level（努力级别）](/en/model-config#adjust-effort-level)。覆盖会话 effort level。默认：继承自会话。可选：`low`、`medium`、`high`、`xhigh`、`max`；可用级别取决于模型。                                                                                                                                                                                                      |
| `context`                  | 否          | 设为 `fork` 可在派生的 subagent 上下文中运行。                                                                                                                                                                                                                                                                                                                                                                                               |
| `agent`                    | 否          | 当设置 `context: fork` 时使用的 subagent 类型。                                                                                                                                                                                                                                                                                                                                                                                          |
| `hooks`                    | 否          | 作用于该 skill 生命周期的 hook。配置格式参见[skill 与 agent 中的 hook](/en/hooks#hooks-in-skills-and-agents)。                                                                                                                                                                                                                                                                                                         |
| `paths`                    | 否          | 限制该 skill 何时激活的 glob 模式。接受逗号分隔字符串或 YAML 列表。设置后，Claude 仅在处理匹配模式的文件时自动加载该 skill。格式与[路径专属规则](/en/memory#path-specific-rules)相同。                                                                                                                                                        |
| `shell`                    | 否          | 该 skill 中 `` !`command` `` 和 ` ```! ` 代码块使用的 shell。接受 `bash`（默认）或 `powershell`。设为 `powershell` 时在 Windows 上通过 PowerShell 运行内联 shell 命令。需要 `CLAUDE_CODE_USE_POWERSHELL_TOOL=1`。                                                                                                                                                                                                          |

#### How a skill gets its command name

The command you type to invoke a skill comes from where the skill file lives. The frontmatter `name` field sets the display label shown in skill listings and, except for a plugin-root `SKILL.md`, does not change what you type after `/`.

#### skill 如何获得命令名

你输入的调用命令取决于 skill 文件所在位置。frontmatter 的 `name` 字段设置 skill 列表中显示的标签，除 plugin 根的 `SKILL.md` 外，它不会改变 `/` 之后输入的内容。

The table below shows where the command name comes from for each layout:

下表展示每种布局下命令名的来源：

| Skill location                                                                                     | Command name source                                                                | Example                                                                                                                              |
| :------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| Skill directory under `~/.claude/skills/` or `.claude/skills/`                                     | Directory name                                                                     | `.claude/skills/deploy-staging/SKILL.md` → `/deploy-staging`                                                                         |
| [Nested](#where-skills-live) `.claude/skills/` directory, when the name clashes with another skill | Subdirectory path relative to the working directory, then the skill directory name | `apps/web/.claude/skills/deploy/SKILL.md` → `/apps/web:deploy`                                                                       |
| File under `.claude/commands/`                                                                     | File name without extension                                                        | `.claude/commands/deploy.md` → `/deploy`                                                                                             |
| Plugin `skills/` subdirectory                                                                      | Directory name, namespaced by plugin                                               | `my-plugin/skills/review/SKILL.md` → `/my-plugin:review`                                                                             |
| Plugin root `SKILL.md`                                                                             | Frontmatter `name`, with the plugin directory name as a fallback                   | `my-plugin/SKILL.md` with `name: review` → `/my-plugin:review`. See [Path behavior rules](/en/plugins-reference#path-behavior-rules) |

| skill 位置                                                                                     | 命令名来源                                                                | 示例                                                                                                                              |
| :------------------------------------------------------------------------------------------------- | :--------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------- |
| `~/.claude/skills/` 或 `.claude/skills/` 下的 skill 目录                                     | 目录名                                                                     | `.claude/skills/deploy-staging/SKILL.md` → `/deploy-staging`                                                                         |
| [嵌套](#where-skills-live) `.claude/skills/` 目录，当名称与其它 skill 冲突时 | 相对工作目录的子目录路径，加上 skill 目录名 | `apps/web/.claude/skills/deploy/SKILL.md` → `/apps/web:deploy`                                                                       |
| `.claude/commands/` 下的文件                                                                     | 不带扩展名的文件名                                                        | `.claude/commands/deploy.md` → `/deploy`                                                                                             |
| Plugin 的 `skills/` 子目录                                                                      | 目录名，以 plugin 为命名空间                                               | `my-plugin/skills/review/SKILL.md` → `/my-plugin:review`                                                                             |
| Plugin 根 `SKILL.md`                                                                             | frontmatter `name`，以 plugin 目录名为回退                   | `my-plugin/SKILL.md` 且 `name: review` → `/my-plugin:review`。参见[路径行为规则](/en/plugins-reference#path-behavior-rules) |

The plugin-root case is the one place where `name` does set the command name, because there is no skill directory to take it from. If `name` is not set in the frontmatter, the plugin's directory name is used instead.

plugin 根是唯一由 `name` 决定命令名的情况，因为没有 skill 目录可取名称。若 frontmatter 中未设置 `name`，则使用 plugin 的目录名。

#### Available string substitutions

Skills support string substitution for dynamic values in the skill content:

#### 可用的字符串替换

skill 支持对 skill 内容中的动态值进行字符串替换：

| Variable                | Description                                                                                                                                                                                                                                                                                                 |
| :---------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$ARGUMENTS`            | All arguments passed when invoking the skill. If `$ARGUMENTS` is not present in the content, arguments are appended as `ARGUMENTS: <value>`.                                                                                                                                                                |
| `$ARGUMENTS[N]`         | Access a specific argument by 0-based index, such as `$ARGUMENTS[0]` for the first argument.                                                                                                                                                                                                                |
| `$N`                    | Shorthand for `$ARGUMENTS[N]`, such as `$0` for the first argument or `$1` for the second.                                                                                                                                                                                                                  |
| `$name`                 | Named argument declared in the [`arguments`](#frontmatter-reference) frontmatter list. Names map to positions in order, so with `arguments: [issue, branch]` the placeholder `$issue` expands to the first argument and `$branch` to the second.                                                            |
| `${CLAUDE_SESSION_ID}`  | The current session ID. Useful for logging, creating session-specific files, or correlating skill output with sessions.                                                                                                                                                                                     |
| `${CLAUDE_EFFORT}`      | The current effort level: `low`, `medium`, `high`, `xhigh`, or `max`. Ultracode is not a distinct level and reports as `xhigh`. Use this to adapt skill instructions to the active effort setting.                                                                                                          |
| `${CLAUDE_SKILL_DIR}`   | The directory containing the skill's `SKILL.md` file. For plugin skills, this is the skill's subdirectory within the plugin, not the plugin root. Use this in bash injection commands to reference scripts or files bundled with the skill, regardless of the current working directory.                    |
| `${CLAUDE_PROJECT_DIR}` | The project root directory. This is the same path [hooks](/en/hooks#reference-scripts-by-path) and MCP servers receive as `CLAUDE_PROJECT_DIR`. Use this to reference project-local scripts or files, such as `${CLAUDE_PROJECT_DIR}/.claude/hooks/helper.sh`, independent of where the skill is installed. |

| 变量                | 说明                                                                                                                                                                                                                                                                                                 |
| :---------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `$ARGUMENTS`            | 调用 skill 时传入的全部参数。若内容中不存在 `$ARGUMENTS`，参数以 `ARGUMENTS: <value>` 形式追加。                                                                                                                                                                |
| `$ARGUMENTS[N]`         | 按 0 起始索引访问特定参数，例如 `$ARGUMENTS[0]` 为第一个参数。                                                                                                                                                                                                                |
| `$N`                    | `$ARGUMENTS[N]` 的简写，例如 `$0` 为第一个参数，`$1` 为第二个。                                                                                                                                                                                                                  |
| `$name`                 | 在 [`arguments`](#frontmatter-reference) frontmatter 列表中声明的具名参数。名称按顺序映射到位置，因此 `arguments: [issue, branch]` 中 `$issue` 展开为第一个参数，`$branch` 为第二个。                                                            |
| `${CLAUDE_SESSION_ID}`  | 当前会话 ID。可用于日志、创建会话专属文件，或将 skill 输出与会话关联。                                                                                                                                                                                     |
| `${CLAUDE_EFFORT}`      | 当前 effort level：`low`、`medium`、`high`、`xhigh` 或 `max`。Ultracode 不是独立级别，报告为 `xhigh`。可用于让 skill 指令适配当前 effort 设置。                                                                                                          |
| `${CLAUDE_SKILL_DIR}`   | 包含该 skill `SKILL.md` 文件的目录。对 plugin skill，这是 plugin 内的 skill 子目录，而非 plugin 根。可在 bash 注入命令中引用与 skill 打包的脚本或文件，与当前工作目录无关。                    |
| `${CLAUDE_PROJECT_DIR}` | 项目根目录。与 [hook](/en/hooks#reference-scripts-by-path) 和 MCP server 收到的 `CLAUDE_PROJECT_DIR` 同路径。可用于引用项目本地脚本或文件，如 `${CLAUDE_PROJECT_DIR}/.claude/hooks/helper.sh`，与 skill 安装位置无关。 |

The `${CLAUDE_PROJECT_DIR}` substitution requires Claude Code v2.1.196 or later. It applies to both the skill body and the [`allowed-tools`](#frontmatter-reference) frontmatter, so a permission rule like `Bash(${CLAUDE_PROJECT_DIR}/scripts/lint.sh *)` resolves to the same path the skill body uses.

`${CLAUDE_PROJECT_DIR}` 替换要求 Claude Code v2.1.196 或更高版本。它同时适用于 skill 正文和 [`allowed-tools`](#frontmatter-reference) frontmatter，因此权限规则 `Bash(${CLAUDE_PROJECT_DIR}/scripts/lint.sh *)` 会解析为与 skill 正文相同的路径。

Indexed arguments use shell-style quoting, so wrap multi-word values in quotes to pass them as a single argument. For example, `/my-skill "hello world" second` makes `$0` expand to `hello world` and `$1` to `second`. The `$ARGUMENTS` placeholder always expands to the full argument string as typed.

带索引的参数使用 shell 风格引用，因此多词值需用引号包裹以作为单个参数传入。例如 `/my-skill "hello world" second` 会使 `$0` 展开为 `hello world`、`$1` 为 `second`。`$ARGUMENTS` 占位符始终展开为所输入的完整参数字符串。

To include a literal `$` before a digit, `ARGUMENTS`, or a declared argument name, such as `$1.00` in prose, escape it with a backslash: `\$1.00`. A backslash before any other `$` is left unchanged. Only a single backslash directly before the token escapes it. A doubled backslash such as `\\$1` leaves both backslashes in place, and `$1` still expands to the argument value.

若要在数字、`ARGUMENTS` 或已声明参数名前包含字面 `$`（如正文中的 `$1.00`），用反斜杠转义：`\$1.00`。其它任何 `$` 前的反斜杠保持不变。仅紧贴 token 前的单个反斜杠会转义。双反斜杠如 `\\$1` 会保留两个反斜杠，且 `$1` 仍展开为参数值。

**Example using substitutions:**

**使用替换的示例：**

```yaml
---
name: session-logger
description: Log activity for this session
---

Log the following to logs/${CLAUDE_SESSION_ID}.log:

$ARGUMENTS
```

### Add supporting files

Skills can include multiple files in their directory. This keeps `SKILL.md` focused on the essentials while letting Claude access detailed reference material only when needed. Large reference docs, API specifications, or example collections don't need to load into context every time the skill runs.

### 添加辅助文件

skill 可在其目录中包含多个文件。这样可让 `SKILL.md` 聚焦要点，而 Claude 仅在需要时访问详细参考资料。大型参考文档、API 规范或示例集合不必在每次 skill 运行时都载入上下文。

```text
my-skill/
├── SKILL.md (required - overview and navigation)
├── reference.md (detailed API docs - loaded when needed)
├── examples.md (usage examples - loaded when needed)
└── scripts/
    └── helper.py (utility script - executed, not loaded)
```

Reference supporting files from `SKILL.md` so Claude knows what each file contains and when to load it:

在 `SKILL.md` 中引用辅助文件，让 Claude 知道每个文件的内容及何时加载：

```markdown
## Additional resources

- For complete API details, see [reference.md](reference.md)
- For usage examples, see [examples.md](examples.md)
```

<Tip>Keep `SKILL.md` under 500 lines. Move detailed reference material to separate files.</Tip>

<Tip>将 `SKILL.md` 控制在 500 行以内。把详细参考资料移到单独文件。</Tip>

### Control who invokes a skill

By default, both you and Claude can invoke any skill. You can type `/skill-name` to invoke it directly, and Claude can load it automatically when relevant to your conversation. Two frontmatter fields let you restrict this:

### 控制由谁调用 skill

默认情况下，你和 Claude 都可调用任何 skill。你可以输入 `/skill-name` 直接调用，Claude 也可在对话相关时自动加载。两个 frontmatter 字段可用于限制：

* **`disable-model-invocation: true`**: Only you can invoke the skill. Use this for workflows with side effects or that you want to control timing, like `/commit`, `/deploy`, or `/send-slack-message`. You don't want Claude deciding to deploy because your code looks ready.

* **`disable-model-invocation: true`**：仅你可调用该 skill。用于有副作用或你想控制时机的工作流，如 `/commit`、`/deploy` 或 `/send-slack-message`。你不希望 Claude 因为代码看起来就绪就自行部署。

* **`user-invocable: false`**: Only Claude can invoke the skill. Use this for background knowledge that isn't actionable as a command. A `legacy-system-context` skill explains how an old system works. Claude should know this when relevant, but `/legacy-system-context` isn't a meaningful action for users to take.

* **`user-invocable: false`**：仅 Claude 可调用该 skill。用于不可作为命令执行的背景知识。`legacy-system-context` skill 解释某旧系统如何运作。Claude 在相关时应知晓，但 `/legacy-system-context` 对用户而言不是有意义的动作。

This example creates a deploy skill that only you can trigger. The `disable-model-invocation: true` field prevents Claude from running it automatically:

本示例创建一个仅你可触发的 deploy skill。`disable-model-invocation: true` 字段阻止 Claude 自动运行：

```yaml
---
name: deploy
description: Deploy the application to production
disable-model-invocation: true
---

Deploy $ARGUMENTS to production:

1. Run the test suite
2. Build the application
3. Push to the deployment target
4. Verify the deployment succeeded
```

Here's how the two fields affect invocation and context loading:

这两个字段对调用与上下文加载的影响如下：

| Frontmatter                      | You can invoke | Claude can invoke | When loaded into context                                     |
| :------------------------------- | :------------- | :---------------- | :----------------------------------------------------------- |
| (default)                        | Yes            | Yes               | Description always in context, full skill loads when invoked |
| `disable-model-invocation: true` | Yes            | No                | Description not in context, full skill loads when you invoke |
| `user-invocable: false`          | No             | Yes               | Description always in context, full skill loads when invoked |

| frontmatter                      | 你可调用 | Claude 可调用 | 何时载入上下文                                     |
| :------------------------------- | :------------- | :---------------- | :----------------------------------------------------------- |
| （默认）                        | 是            | 是               | description 始终在上下文中，完整 skill 在调用时加载 |
| `disable-model-invocation: true` | 是            | 否                | description 不在上下文中，完整 skill 在你调用时加载 |
| `user-invocable: false`          | 否             | 是               | description 始终在上下文中，完整 skill 在调用时加载 |

<Note>
In a regular session, skill descriptions are loaded into context so Claude knows what's available, but full skill content only loads when invoked. [Subagents with preloaded skills](/en/sub-agents#preload-skills-into-subagents) work differently: the full skill content is injected at startup.
</Note>

<Note>
在常规会话中，skill 的 description 会载入上下文以便 Claude 知晓可用项，但完整 skill 内容仅在调用时加载。[预加载 skill 的 subagent](/en/sub-agents#preload-skills-into-subagents) 不同：完整 skill 内容在启动时即注入。
</Note>

### Skill content lifecycle

When you or Claude invoke a skill, the rendered `SKILL.md` content enters the conversation as a single message and stays there for the rest of the session. Claude Code does not re-read the skill file on later turns, so write guidance that should apply throughout a task as standing instructions rather than one-time steps.

### skill 内容生命周期

当你或 Claude 调用 skill 时，渲染后的 `SKILL.md` 内容作为单条消息进入对话，并在会话剩余部分持续存在。Claude Code 不会在后续轮次重新读取 skill 文件，因此应将需要贯穿任务的指引写成常驻指令，而非一次性步骤。

[Auto-compaction](/en/how-claude-code-works#when-context-fills-up) carries invoked skills forward within a token budget. When the conversation is summarized to free context, Claude Code re-attaches the most recent invocation of each skill after the summary, keeping the first 5,000 tokens of each. Re-attached skills share a combined budget of 25,000 tokens. Claude Code fills this budget starting from the most recently invoked skill, so older skills can be dropped entirely after compaction if you have invoked many in one session.

[自动压缩](/en/how-claude-code-works#when-context-fills-up)会在 token 预算内把已调用 skill 向前携带。当对话被摘要以释放上下文时，Claude Code 会在摘要后重新附上每个 skill 最近一次调用，保留各自前 5,000 个 token。重新附上的 skill 共享 25,000 token 的合并预算。Claude Code 从最近调用的 skill 开始填充该预算，因此若一次会话中调用了很多 skill，较早的可能在压缩后被完全丢弃。

If a skill seems to stop influencing behavior after the first response, the content is usually still present and the model is choosing other tools or approaches. Strengthen the skill's `description` and instructions so the model keeps preferring it, or use [hooks](/en/hooks) to enforce behavior deterministically. If the skill is large or you invoked several others after it, re-invoke it after compaction to restore the full content.

若 skill 在首次回复后似乎不再影响行为，通常内容仍在，只是模型选择了其它工具或方式。强化该 skill 的 `description` 和指令，使模型持续优先它；或用 [hook](/en/hooks) 确定性地强制行为。若 skill 较大或之后又调用了多个其它 skill，可在压缩后重新调用以恢复完整内容。

### Pre-approve tools for a skill

The `allowed-tools` field grants permission for the listed tools while the skill is active, so Claude can use them without prompting you for approval. It does not restrict which tools are available: every tool remains callable, and your [permission settings](/en/permissions) still govern tools that are not listed.

### 为 skill 预批准工具

`allowed-tools` 字段在该 skill 激活期间为所列工具授予许可，使 Claude 无需提示你批准即可使用。它不限制可用工具：每个工具仍可调用，未列出工具仍由你的[权限设置](/en/permissions)管控。

For skills checked into a project's `.claude/skills/` directory, `allowed-tools` takes effect after you accept the workspace trust dialog for that folder, the same as permission rules in `.claude/settings.json`. Review project skills before trusting a repository, since a skill can grant itself broad tool access.

对于提交到项目 `.claude/skills/` 目录的 skill，`allowed-tools` 在你接受该文件夹的 workspace 信任对话框后生效，与 `.claude/settings.json` 中的权限规则一致。在信任仓库前应审查项目 skill，因为 skill 可为自己授予广泛的工具访问权。

This skill lets Claude run git commands without per-use approval whenever you invoke it:

该 skill 让 Claude 在你调用时无需逐次批准即可运行 git 命令：

```yaml
---
name: commit
description: Stage and commit the current changes
disable-model-invocation: true
allowed-tools: Bash(git add *) Bash(git commit *) Bash(git status *)
---
```

To remove tools from Claude's available pool while a skill is active, list them in `disallowed-tools` in the skill's frontmatter. The restriction clears when you send your next message. To block tools across all skills and prompts, add deny rules in your [permission settings](/en/permissions).

要在 skill 激活时从 Claude 可用池中移除工具，在 skill 的 frontmatter 中用 `disallowed-tools` 列出。限制在你发送下一条消息时清除。若要在所有 skill 和 prompt 中屏蔽工具，在[权限设置](/en/permissions)中添加 deny 规则。

### Pass arguments to skills

Both you and Claude can pass arguments when invoking a skill. Arguments are available via the `$ARGUMENTS` placeholder.

### 向 skill 传参

你和 Claude 在调用 skill 时都可传参。参数通过 `$ARGUMENTS` 占位符获取。

This skill fixes a GitHub issue by number. The `$ARGUMENTS` placeholder gets replaced with whatever follows the skill name:

该 skill 按编号修复 GitHub issue。`$ARGUMENTS` 占位符会被 skill 名称之后的内容替换：

```yaml
---
name: fix-issue
description: Fix a GitHub issue
disable-model-invocation: true
---

Fix GitHub issue $ARGUMENTS following our coding standards.

1. Read the issue description
2. Understand the requirements
3. Implement the fix
4. Write tests
5. Create a commit
```

When you run `/fix-issue 123`, Claude receives "Fix GitHub issue 123 following our coding standards..."

运行 `/fix-issue 123` 时，Claude 收到 "Fix GitHub issue 123 following our coding standards..."。

If you invoke a skill with arguments but the skill doesn't include `$ARGUMENTS`, Claude Code appends `ARGUMENTS: <your input>` to the end of the skill content so Claude still sees what you typed.

若你带参数调用 skill 但 skill 中不含 `$ARGUMENTS`，Claude Code 会在 skill 内容末尾追加 `ARGUMENTS: <your input>`，让 Claude 仍能看到你的输入。

You can also stack several skills at the start of one message. As of v2.1.199, typing `/code-review /fix-issue 123` loads both skills and passes the trailing text `123` as `$ARGUMENTS` to each of them. In earlier versions, only the first skill loaded and received `/fix-issue 123` as literal argument text.

你也可以在一条消息开头堆叠多个 skill。自 v2.1.199 起，输入 `/code-review /fix-issue 123` 会同时加载两个 skill，并把尾随文本 `123` 作为 `$ARGUMENTS` 传给每个 skill。早期版本中，仅第一个 skill 被加载，且 `/fix-issue 123` 作为字面参数文本传入。

Claude Code expands the first skill plus up to five more stacked after it. Expansion stops at the first token that isn't an inline user-invocable skill, so a skill that runs as a [forked subagent](#run-skills-in-a-subagent) or one whose arguments may themselves start with a slash command, such as `/loop`, also ends the run there; that token and everything after it become the argument text for every expanded skill.

Claude Code 会展开第一个 skill 以及其后堆叠的最多五个 skill。展开在遇到第一个不是内联用户可调用 skill 的 token 时停止；因此以[派生 subagent](#run-skills-in-a-subagent) 运行的 skill，或其参数本身可能以斜杠命令（如 `/loop`）开头的 skill，也会在此终止展开；该 token 及其后的所有内容成为每个已展开 skill 的参数文本。

To access individual arguments by position, use `$ARGUMENTS[N]` or the shorter `$N`:

要按位置访问单个参数，使用 `$ARGUMENTS[N]` 或简写 `$N`：

```yaml
---
name: migrate-component
description: Migrate a component from one framework to another
---

Migrate the $ARGUMENTS[0] component from $ARGUMENTS[1] to $ARGUMENTS[2].
Preserve all existing behavior and tests.
```

Running `/migrate-component SearchBar React Vue` replaces `$ARGUMENTS[0]` with `SearchBar`, `$ARGUMENTS[1]` with `React`, and `$ARGUMENTS[2]` with `Vue`. The same skill using the `$N` shorthand:

运行 `/migrate-component SearchBar React Vue` 会把 `$ARGUMENTS[0]` 替换为 `SearchBar`、`$ARGUMENTS[1]` 替换为 `React`、`$ARGUMENTS[2]` 替换为 `Vue`。使用 `$N` 简写的同一 skill：

```yaml
---
name: migrate-component
description: Migrate a component from one framework to another
---

Migrate the $0 component from $1 to $2.
Preserve all existing behavior and tests.
```

### Advanced patterns

#### Inject dynamic context

The `` !`<command>` `` syntax runs shell commands before the skill content is sent to Claude. The command output replaces the placeholder, so Claude receives actual data, not the command itself.

### 高级模式

#### 注入动态上下文

`` !`<command>` `` 语法在 skill 内容发送给 Claude 之前运行 shell 命令。命令输出替换占位符，因此 Claude 收到的是实际数据，而非命令本身。

This skill summarizes a pull request by fetching live PR data with the GitHub CLI. The `` !`gh pr diff` `` and other commands run first, and their output gets inserted into the prompt:

该 skill 通过 GitHub CLI 获取实时 PR 数据来总结 pull request（合并请求）。`` !`gh pr diff` `` 等命令先运行，其输出被插入 prompt：

```yaml
---
name: pr-summary
description: Summarize changes in a pull request
context: fork
agent: Explore
allowed-tools: Bash(gh *)
---

## Pull request context
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
- Changed files: !`gh pr diff --name-only`

## Your task
Summarize this pull request...
```

When this skill runs:

该 skill 运行时：

1. Each `` !`<command>` `` executes immediately (before Claude sees anything)
2. The output replaces the placeholder in the skill content
3. Claude receives the fully-rendered prompt with actual PR data

1. 每个 `` !`<command>` `` 立即执行（在 Claude 看到任何内容之前）
2. 输出替换 skill 内容中的占位符
3. Claude 收到带实际 PR 数据的完整渲染 prompt

This is preprocessing, not something Claude executes. Claude only sees the final result.

这是预处理，并非 Claude 执行。Claude 只看到最终结果。

Substitution runs once over the original file. Command output is inserted as plain text and is not re-scanned for further `` !`<command>` `` placeholders, so a command cannot emit a placeholder for a later pass to expand.

替换对原始文件只运行一次。命令输出以纯文本插入，不会再次扫描其中的 `` !`<command>` `` 占位符，因此命令不能为后续轮次输出占位符。

The inline form is only recognized when `!` appears at the start of a line or immediately after whitespace. If `!` follows another character, as in `` KEY=!`cmd` ``, the placeholder is left as literal text and the command does not run.

仅当 `!` 出现在行首或紧跟空白之后时，内联形式才被识别。若 `!` 紧跟其它字符（如 `` KEY=!`cmd` ``），占位符保留为字面文本，命令不运行。

For multi-line commands, use a fenced code block opened with ` ```! ` instead of the inline form:

对于多行命令，使用以 ` ```! ` 开头的围栏代码块，而非内联形式：

````markdown
## Environment

```!
node --version
npm --version
git status --short
```
````

To disable this behavior for skills and custom commands from user, project, plugin, or [additional-directory](#skills-from-additional-directories) sources, set `"disableSkillShellExecution": true` in [settings](/en/settings). Each command is replaced with `[shell command execution disabled by policy]` instead of being run. Bundled and managed skills are not affected. This setting is most useful in [managed settings](/en/permissions#managed-settings), where users cannot override it.

若要针对来自 user、project、plugin 或[额外目录](#skills-from-additional-directories)来源的 skill 和自定义命令禁用此行为，在[设置](/en/settings)中设 `"disableSkillShellExecution": true`。每条命令会被替换为 `[shell command execution disabled by policy]` 而非运行。内置和 managed skill 不受影响。此设置在 [managed settings](/en/permissions#managed-settings) 中最有用，因为用户无法覆盖。

<Tip>
To request deeper reasoning when a skill runs, include `ultrathink` anywhere in the skill content. See [Use ultrathink for one-off deep reasoning](/en/model-config#use-ultrathink-for-one-off-deep-reasoning).
</Tip>

<Tip>
若要在 skill 运行时请求更深层推理，在 skill 内容任意位置加入 `ultrathink`。参见[使用 ultrathink 进行一次性深度推理](/en/model-config#use-ultrathink-for-one-off-deep-reasoning)。
</Tip>

### Run skills in a subagent

Add `context: fork` to your frontmatter when you want a skill to run in isolation. The skill content becomes the prompt that drives the subagent. It won't have access to your conversation history.

### 在 subagent 中运行 skill

当你希望 skill 隔离运行时，在 frontmatter 中添加 `context: fork`。skill 内容成为驱动 subagent 的 prompt。它无法访问你的对话历史。

<Warning>
`context: fork` only makes sense for skills with explicit instructions. If your skill contains guidelines like "use these API conventions" without a task, the subagent receives the guidelines but no actionable prompt, and returns without meaningful output.
</Warning>

<Warning>
`context: fork` 仅适用于带明确指令的 skill。若你的 skill 只含"使用这些 API 约定"之类的指引而无任务，subagent 收到指引但没有可执行的 prompt，会无有意义输出地返回。
</Warning>

Skills and [subagents](/en/sub-agents) work together in two directions:

skill 与 [subagent](/en/sub-agents) 双向协作：

| Approach                     | System prompt            | Task                        | Also loads                                          |
| :--------------------------- | :----------------------- | :-------------------------- | :-------------------------------------------------- |
| Skill with `context: fork`   | From agent type          | SKILL.md content            | CLAUDE.md, except when the agent is Explore or Plan |
| Subagent with `skills` field | Subagent's markdown body | Claude's delegation message | Preloaded skills + CLAUDE.md                        |

| 方式                     | System prompt            | 任务                        | 另外加载                                          |
| :--------------------------- | :----------------------- | :-------------------------- | :-------------------------------------------------- |
| 带 `context: fork` 的 skill   | 来自 agent 类型          | SKILL.md 内容            | CLAUDE.md，但当 agent 为 Explore 或 Plan 时除外 |
| 带 `skills` 字段的 subagent | subagent 的 markdown 正文 | Claude 的委派消息 | 预加载 skill + CLAUDE.md                        |

With `context: fork`, you write the task in your skill and pick an agent type to execute it. The built-in Explore and Plan agents [skip CLAUDE.md and git status](/en/sub-agents#what-loads-at-startup) to keep their context small, so a forked skill using `agent: Explore` sees only the SKILL.md content and the agent's own system prompt. For the inverse, where you define a custom subagent that uses skills as reference material, see [Subagents](/en/sub-agents#preload-skills-into-subagents).

使用 `context: fork` 时，你在 skill 中编写任务并选择一个 agent 类型执行。内置 Explore 和 Plan agent 会[跳过 CLAUDE.md 和 git status](/en/sub-agents#what-loads-at-startup) 以保持上下文精简，因此使用 `agent: Explore` 的派生 skill 只看到 SKILL.md 内容和 agent 自身的 system prompt。反之，若你定义一个使用 skill 作为参考材料的自定义 subagent，参见 [Subagents](/en/sub-agents#preload-skills-into-subagents)。

#### Example: Research skill using Explore agent

This skill runs research in a forked Explore agent. The skill content becomes the task, and the agent provides read-only tools optimized for codebase exploration:

#### 示例：使用 Explore agent 的研究型 skill

该 skill 在派生的 Explore agent 中运行研究。skill 内容成为任务，agent 提供为代码库探索优化的只读工具：

```yaml
---
name: deep-research
description: Research a topic thoroughly
context: fork
agent: Explore
---

Research $ARGUMENTS thoroughly:

1. Find relevant files using Glob and Grep
2. Read and analyze the code
3. Summarize findings with specific file references
```

When this skill runs:

该 skill 运行时：

1. A new isolated context is created
2. The subagent receives the skill content as its prompt ("Research \$ARGUMENTS thoroughly...")
3. The `agent` field determines the execution environment (model, tools, and permissions)
4. Results are summarized and returned to your main conversation

1. 创建新的隔离上下文
2. subagent 收到 skill 内容作为 prompt（"Research \$ARGUMENTS thoroughly..."）
3. `agent` 字段决定执行环境（模型、工具和权限）
4. 结果被摘要并返回主对话

The `agent` field specifies which subagent configuration to use. Options include built-in agents (`Explore`, `Plan`, `general-purpose`) or any custom subagent from `.claude/agents/`. If omitted, uses `general-purpose`.

`agent` 字段指定使用哪个 subagent 配置。可选内置 agent（`Explore`、`Plan`、`general-purpose`）或 `.claude/agents/` 中的任何自定义 subagent。若省略，使用 `general-purpose`。

### Restrict Claude's skill access

By default, Claude can invoke any skill that doesn't have `disable-model-invocation: true` set. Skills that define `allowed-tools` grant Claude access to those tools without per-use approval when the skill is active. Your [permission settings](/en/permissions) still govern baseline approval behavior for all other tools. A few built-in commands are also available through the Skill tool, including `/init`, `/review`, and `/security-review`. Other built-in commands such as `/compact` are not.

### 限制 Claude 的 skill 访问

默认情况下，Claude 可调用任何未设置 `disable-model-invocation: true` 的 skill。定义了 `allowed-tools` 的 skill 在激活时授予 Claude 免逐次批准使用这些工具的权限。你的[权限设置](/en/permissions)仍管控所有其它工具的基础批准行为。少数内置命令也可通过 Skill 工具使用，包括 `/init`、`/review` 和 `/security-review`。`/compact` 等其它内置命令则不可。

Three ways to control which skills Claude can invoke:

控制 Claude 可调用哪些 skill 的三种方式：

**Disable all skills** by denying the Skill tool in `/permissions`:

**禁用所有 skill**，在 `/permissions` 中 deny Skill 工具：

```text
# Add to deny rules:
Skill
```

**Allow or deny specific skills** using [permission rules](/en/permissions):

**允许或拒绝特定 skill**，使用[权限规则](/en/permissions)：

```text
# Allow only specific skills
Skill(commit)
Skill(review-pr *)

# Deny specific skills
Skill(deploy *)
```

Permission syntax: `Skill(name)` for exact match, `Skill(name *)` for prefix match with any arguments.

权限语法：`Skill(name)` 为精确匹配，`Skill(name *)` 为带任意参数的前缀匹配。

**Hide individual skills** by adding `disable-model-invocation: true` to their frontmatter. This removes the skill from Claude's context entirely.

**隐藏单个 skill**，在其 frontmatter 中添加 `disable-model-invocation: true`。这会从 Claude 的上下文中完全移除该 skill。

<Note>
The `user-invocable` field only controls menu visibility, not Skill tool access. Use `disable-model-invocation: true` to block programmatic invocation.
</Note>

<Note>
`user-invocable` 字段仅控制菜单可见性，不影响 Skill 工具访问。要阻止程序化调用，使用 `disable-model-invocation: true`。
</Note>

### Override skill visibility from settings

The `skillOverrides` setting controls skill visibility from your [settings](/en/settings) instead of the skill's own frontmatter. Use it for skills whose SKILL.md you don't want to edit, such as ones checked into a shared project repo or provided by an MCP server. The `/skills` menu writes it for you: highlight a skill and press `Space` to cycle states, then `Enter` to save to `.claude/settings.local.json`.

### 从设置覆盖 skill 可见性

`skillOverrides` 设置从你的[设置](/en/settings)而非 skill 自身 frontmatter 控制 skill 可见性。用于你不想修改其 SKILL.md 的 skill，如提交到共享项目仓库或由 MCP server 提供的 skill。`/skills` 菜单可代你写入：高亮某个 skill 并按 `Space` 循环状态，再按 `Enter` 保存到 `.claude/settings.local.json`。

Each key is a skill name and each value is one of four states:

每个键为 skill 名称，每个值为四种状态之一：

| Value                   | Listed to Claude     | In `/` menu |
| :---------------------- | :------------------- | :---------- |
| `"on"`                  | Name and description | Yes         |
| `"name-only"`           | Name only            | Yes         |
| `"user-invocable-only"` | Hidden               | Yes         |
| `"off"`                 | Hidden               | Hidden      |

| 值                   | 对 Claude 列出     | 在 `/` 菜单中 |
| :---------------------- | :------------------- | :---------- |
| `"on"`                  | 名称和 description | 是         |
| `"name-only"`           | 仅名称            | 是         |
| `"user-invocable-only"` | 隐藏               | 是         |
| `"off"`                 | 隐藏               | 隐藏      |

As of v2.1.199, `"off"` also hides the skill from the command lists advertised to [Remote Control](/en/remote-control) clients and to [Agent SDK](/en/agent-sdk/slash-commands) callers, not only the terminal `/` menu. Invoking a hidden skill by its full name still returns the `skillOverrides` error instead of running it.

自 v2.1.199 起，`"off"` 还会对 [Remote Control](/en/remote-control) 客户端和 [Agent SDK](/en/agent-sdk/slash-commands) 调用方通告的命令列表隐藏该 skill，不仅是终端 `/` 菜单。以全名调用被隐藏的 skill 仍会返回 `skillOverrides` 错误而非运行。

A skill that is absent from `skillOverrides` is treated as `"on"`. The example below collapses one skill to its name and turns another off entirely:

不在 `skillOverrides` 中的 skill 视为 `"on"`。下例将一个 skill 折叠为仅名称，将另一个完全关闭：

```json
{
  "skillOverrides": {
    "legacy-context": "name-only",
    "deploy": "off"
  }
}
```

Plugin skills are not affected by `skillOverrides`. Manage those through `/plugin` instead.

Plugin skill 不受 `skillOverrides` 影响。请通过 `/plugin` 管理。

### Evaluate and iterate on a skill

Seeing a skill trigger tells you Claude found it, not that it did what you intended. To know a skill is working, measure two things separately: whether Claude invokes it on the prompts it should, and whether the output matches what you expect when it does.

### 评估并迭代 skill

看到 skill 被触发只说明 Claude 找到了它，并不代表它达到了你的预期。要确认 skill 有效，需分别衡量两点：Claude 是否在该触发的 prompt 上触发它，以及触发时输出是否符合预期。

The check for both is a baseline comparison. Collect a few realistic prompts, run each one in a fresh session with the skill available and again with it [disabled](#override-skill-visibility-from-settings), and compare the results. A fresh session matters because leftover context from authoring the skill will mask gaps in the written instructions.

两点的检验都是基线对比。收集几个真实 prompt，在全新会话中分别在 skill 可用与[禁用](#override-skill-visibility-from-settings)下各运行一次，比较结果。全新会话很重要，因为编写 skill 时残留的上下文会掩盖书面指令的不足。

#### Run evals with skill-creator

The [`skill-creator` plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) automates the comparison loop inside Claude Code. Install it from the official marketplace:

#### 用 skill-creator 运行评测

[`skill-creator` plugin](https://github.com/anthropics/claude-plugins-official/tree/main/plugins/skill-creator) 在 Claude Code 内自动完成对比循环。从官方 marketplace 安装：

```text
/plugin install skill-creator@claude-plugins-official
```

If Claude Code reports that the plugin is not found in any marketplace, your marketplace is either missing or outdated. Run `/plugin marketplace update claude-plugins-official` to refresh it, or `/plugin marketplace add anthropics/claude-plugins-official` if you haven't added it before. Then retry the install.

若 Claude Code 报告在任何 marketplace 中都找不到该 plugin，说明你的 marketplace 缺失或过时。运行 `/plugin marketplace update claude-plugins-official` 刷新，或若之前未添加过则运行 `/plugin marketplace add anthropics/claude-plugins-official`。然后重试安装。

After installing, run `/reload-plugins` to make the plugin's skills available in the current session. Then ask Claude to evaluate an existing skill, for example `evaluate my summarize-changes skill with skill-creator`. The plugin walks you through writing test cases and runs the loop:

安装后，运行 `/reload-plugins` 使该 plugin 的 skill 在当前会话可用。然后让 Claude 评估现有 skill，例如 `evaluate my summarize-changes skill with skill-creator`。该 plugin 会引导你编写测试用例并运行循环：

* **Test cases**: stores prompts, input files, and expected behavior in `evals/evals.json` inside the skill directory
* **Isolated runs**: spawns a [subagent](/en/sub-agents) per test case so each run starts with a clean context, and records token count and duration
* **Grading**: checks each assertion against the output and writes pass or fail with evidence to `grading.json`
* **Benchmark**: aggregates pass rate, time, and tokens for with-skill versus without-skill into `benchmark.json` so you can compare the pass-rate improvement against the token and time overhead
* **Version comparison**: runs a blind A/B between two versions of the skill so you can confirm an edit is an improvement before committing it
* **Description tuning**: generates should-trigger and should-not-trigger prompts, measures the hit rate, and proposes description edits when the skill activates on the wrong requests
* **Review viewer**: opens an HTML report where you inspect each output and record qualitative feedback that the next iteration reads

* **测试用例**：将 prompt、输入文件和预期行为存入 skill 目录内的 `evals/evals.json`
* **隔离运行**：每个测试用例派生一个 [subagent](/en/sub-agents)，使每次运行以干净上下文开始，并记录 token 数和耗时
* **评分**：针对输出检查每条断言，将通过/失败及证据写入 `grading.json`
* **基准**：将启用 skill 与未启用 skill 的通过率、时间、token 汇总到 `benchmark.json`，便于比较通过率提升与 token、时间开销
* **版本对比**：在两个 skill 版本间进行盲测 A/B，以便在提交前确认某次编辑确为改进
* **description 调优**：生成应触发与不应触发的 prompt，测量命中率，并在 skill 错误激活时提出 description 修改建议
* **评审查看器**：打开 HTML 报告，你可检查每个输出并记录定性反馈，供下一轮迭代读取

For the eval file format and the full iteration workflow, see [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills) on agentskills.io. For background on the benchmark and comparison modes, see the [skill-creator announcement](https://claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills).

关于 eval 文件格式和完整迭代工作流，参见 agentskills.io 上的 [Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills)。关于基准和对比模式的背景，参见 [skill-creator 公告](https://claude.com/blog/improving-skill-creator-test-measure-and-refine-agent-skills)。

### Share skills

Skills can be distributed at different scopes depending on your audience:

### 分享 skill

skill 可按不同范围分发，取决于受众：

* **Project skills**: Commit `.claude/skills/` to version control
* **Plugins**: Create a `skills/` directory in your [plugin](/en/plugins)
* **Managed**: Deploy organization-wide through [managed settings](/en/settings#settings-files)

* **项目 skill**：将 `.claude/skills/` 提交到版本控制
* **Plugin**：在你的 [plugin](/en/plugins) 中创建 `skills/` 目录
* **Managed（托管）**：通过 [managed settings](/en/settings#settings-files) 在全组织部署

#### Generate visual output

Skills can bundle and run scripts in any language, giving Claude capabilities beyond what's possible in a single prompt. One powerful pattern is generating visual output: interactive HTML files that open in your browser for exploring data, debugging, or creating reports.

#### 生成可视化输出

skill 可打包并运行任意语言的脚本，赋予 Claude 单个 prompt 无法实现的能力。一种强大模式是生成可视化输出：在浏览器中打开的交互式 HTML 文件，用于探索数据、调试或创建报告。

This example creates a codebase explorer: an interactive tree view where you can expand and collapse directories, see file sizes at a glance, and identify file types by color.

本示例创建一个代码库浏览器：一个交互式树形视图，可展开/折叠目录、一目了然地查看文件大小，并按颜色识别文件类型。

Create the Skill directory:

创建 Skill 目录：

```bash
mkdir -p ~/.claude/skills/codebase-visualizer/scripts
```

Save this to `~/.claude/skills/codebase-visualizer/SKILL.md`. The description tells Claude when to activate this Skill, and the instructions tell Claude to run the bundled script. The script path uses [`${CLAUDE_SKILL_DIR}`](#available-string-substitutions) so it resolves correctly whether the skill is installed at the personal, project, or plugin level:

将其保存到 `~/.claude/skills/codebase-visualizer/SKILL.md`。description 告诉 Claude 何时激活该 Skill，指令告诉 Claude 运行打包的脚本。脚本路径使用 [`${CLAUDE_SKILL_DIR}`](#available-string-substitutions)，无论 skill 安装在 personal、project 还是 plugin 级别都能正确解析：

````yaml
---
name: codebase-visualizer
description: Generate an interactive collapsible tree visualization of your codebase. Use when exploring a new repo, understanding project structure, or identifying large files.
allowed-tools: Bash(python3 *)
---

# Codebase Visualizer

Generate an interactive HTML tree view that shows your project's file structure with collapsible directories.

## Usage

Run the visualization script from your project root:

```bash
python3 ${CLAUDE_SKILL_DIR}/scripts/visualize.py .
```

This creates `codebase-map.html` in the current directory and opens it in your default browser.

## What the visualization shows

- **Collapsible directories**: Click folders to expand/collapse
- **File sizes**: Displayed next to each file
- **Colors**: Different colors for different file types
- **Directory totals**: Shows aggregate size of each folder
````

Save this to `~/.claude/skills/codebase-visualizer/scripts/visualize.py`. This script scans a directory tree and generates a self-contained HTML file with:

将其保存到 `~/.claude/skills/codebase-visualizer/scripts/visualize.py`。该脚本扫描目录树并生成一个自包含 HTML 文件，包含：

* A **summary sidebar** showing file count, directory count, total size, and number of file types
* A **bar chart** breaking down the codebase by file type (top 8 by size)
* A **collapsible tree** where you can expand and collapse directories, with color-coded file type indicators

* 一个**摘要侧边栏**，显示文件数、目录数、总大小和文件类型数
* 一个**柱状图**，按文件类型（按大小前 8）分解代码库
* 一个**可折叠树**，可展开/折叠目录，带有按文件类型着色的指示

The script requires Python 3 but uses only built-in libraries, so there are no packages to install:

该脚本需要 Python 3，但仅使用内置库，无需安装任何包：

```python
#!/usr/bin/env python3
"""Generate an interactive collapsible tree visualization of a codebase."""

import json
import sys
import webbrowser
from html import escape
from pathlib import Path
from collections import Counter

IGNORE = {'.git', 'node_modules', '__pycache__', '.venv', 'venv', 'dist', 'build'}

def scan(path: Path, stats: dict) -> dict:
    result = {"name": path.name, "children": [], "size": 0}
    try:
        for item in sorted(path.iterdir()):
            if item.name in IGNORE or item.name.startswith('.'):
                continue
            if item.is_file():
                size = item.stat().st_size
                ext = item.suffix.lower() or '(no ext)'
                result["children"].append({"name": item.name, "size": size, "ext": ext})
                result["size"] += size
                stats["files"] += 1
                stats["extensions"][ext] += 1
                stats["ext_sizes"][ext] += size
            elif item.is_dir():
                stats["dirs"] += 1
                child = scan(item, stats)
                if child["children"]:
                    result["children"].append(child)
                    result["size"] += child["size"]
    except PermissionError:
        pass
    return result

def generate_html(data: dict, stats: dict, output: Path) -> None:
    ext_sizes = stats["ext_sizes"]
    total_size = sum(ext_sizes.values()) or 1
    sorted_exts = sorted(ext_sizes.items(), key=lambda x: -x[1])[:8]
    colors = {
        '.js': '#f7df1e', '.ts': '#3178c6', '.py': '#3776ab', '.go': '#00add8',
        '.rs': '#dea584', '.rb': '#cc342d', '.css': '#264de4', '.html': '#e34c26',
        '.json': '#6b7280', '.md': '#083fa1', '.yaml': '#cb171e', '.yml': '#cb171e',
        '.mdx': '#083fa1', '.tsx': '#3178c6', '.jsx': '#61dafb', '.sh': '#4eaa25',
    }
    lang_bars = "".join(
        f'<div class="bar-row"><span class="bar-label">{ext}</span>'
        f'<div class="bar" style="width:{(size/total_size)*100}%;background:{colors.get(ext,"#6b7280")}"></div>'
        f'<span class="bar-pct">{(size/total_size)*100:.1f}%</span></div>'
        for ext, size in sorted_exts
    )
    def fmt(b):
        if b < 1024: return f"{b} B"
        if b < 1048576: return f"{b/1024:.1f} KB"
        return f"{b/1048576:.1f} MB"

    html = f'''<!DOCTYPE html>
<html><head>
  <meta charset="utf-8"><title>Codebase Explorer</title>
  <style>
    body {{ font: 14px/1.5 system-ui, sans-serif; margin: 0; background: #1a1a2e; color: #eee; }}
    .container {{ display: flex; height: 100vh; }}
    .sidebar {{ width: 280px; background: #252542; padding: 20px; border-right: 1px solid #3d3d5c; overflow-y: auto; flex-shrink: 0; }}
    .main {{ flex: 1; padding: 20px; overflow-y: auto; }}
    h1 {{ margin: 0 0 10px 0; font-size: 18px; }}
    h2 {{ margin: 20px 0 10px 0; font-size: 14px; color: #888; text-transform: uppercase; }}
    .stat {{ display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #3d3d5c; }}
    .stat-value {{ font-weight: bold; }}
    .bar-row {{ display: flex; align-items: center; margin: 6px 0; }}
    .bar-label {{ width: 55px; font-size: 12px; color: #aaa; }}
    .bar {{ height: 18px; border-radius: 3px; }}
    .bar-pct {{ margin-left: 8px; font-size: 12px; color: #666; }}
    .tree {{ list-style: none; padding-left: 20px; }}
    details {{ cursor: pointer; }}
    summary {{ padding: 4px 8px; border-radius: 4px; }}
    summary:hover {{ background: #2d2d44; }}
    .folder {{ color: #ffd700; }}
    .file {{ display: flex; align-items: center; padding: 4px 8px; border-radius: 4px; }}
    .file:hover {{ background: #2d2d44; }}
    .size {{ color: #888; margin-left: auto; font-size: 12px; }}
    .dot {{ width: 8px; height: 8px; border-radius: 50%; margin-right: 8px; }}
  </style>
</head><body>
  <div class="container">
    <div class="sidebar">
      <h1>📊 Summary</h1>
      <div class="stat"><span>Files</span><span class="stat-value">{stats["files"]:,}</span></div>
      <div class="stat"><span>Directories</span><span class="stat-value">{stats["dirs"]:,}</span></div>
      <div class="stat"><span>Total size</span><span class="stat-value">{fmt(data["size"])}</span></div>
      <div class="stat"><span>File types</span><span class="stat-value">{len(stats["extensions"])}</span></div>
      <h2>By file type</h2>
      {lang_bars}
    </div>
    <div class="main">
      <h1>📁 {escape(data["name"])}</h1>
      <ul class="tree" id="root"></ul>
    </div>
  </div>
  <script>
    const data = {json.dumps(data)};
    const colors = {json.dumps(colors)};
    function fmt(b) {{ if (b < 1024) return b + ' B'; if (b < 1048576) return (b/1024).toFixed(1) + ' KB'; return (b/1048576).toFixed(1) + ' MB'; }}
    function esc(s) {{ return s.replace(/[&<>"']/g, c => ({{"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}}[c])); }}
    function render(node, parent) {{
      if (node.children) {{
        const det = document.createElement('details');
        det.open = parent === document.getElementById('root');
        det.innerHTML = `<summary><span class="folder">📁 ${{esc(node.name)}}</span><span class="size">${{fmt(node.size)}}</span></summary>`;
        const ul = document.createElement('ul'); ul.className = 'tree';
        node.children.sort((a,b) => (b.children?1:0)-(a.children?1:0) || a.name.localeCompare(b.name));
        node.children.forEach(c => render(c, ul));
        det.appendChild(ul);
        const li = document.createElement('li'); li.appendChild(det); parent.appendChild(li);
      }} else {{
        const li = document.createElement('li'); li.className = 'file';
        li.innerHTML = `<span class="dot" style="background:${{colors[node.ext]||'#6b7280'}}"></span>${{esc(node.name)}}<span class="size">${{fmt(node.size)}}</span>`;
        parent.appendChild(li);
      }}
    }}
    data.children.forEach(c => render(c, document.getElementById('root')));
  </script>
</body></html>'''
    output.write_text(html)

if __name__ == '__main__':
    target = Path(sys.argv[1] if len(sys.argv) > 1 else '.').resolve()
    stats = {"files": 0, "dirs": 0, "extensions": Counter(), "ext_sizes": Counter()}
    data = scan(target, stats)
    out = Path('codebase-map.html')
    generate_html(data, stats, out)
    print(f'Generated {out.absolute()}')
    webbrowser.open(f'file://{out.absolute()}')
```

To test, open Claude Code in any project and ask "Visualize this codebase." Claude runs the script, generates `codebase-map.html`, and opens it in your browser.

测试时，在任意项目中打开 Claude Code 并询问 "Visualize this codebase."。Claude 会运行脚本、生成 `codebase-map.html` 并在浏览器中打开。

This pattern works for any visual output: dependency graphs, test coverage reports, API documentation, or database schema visualizations. The bundled script does the work while Claude handles orchestration.

此模式适用于任何可视化输出：依赖图、测试覆盖率报告、API 文档或数据库 schema 可视化。打包的脚本负责具体工作，Claude 负责编排。

### Troubleshooting

#### Skill not triggering

If Claude doesn't use your skill when expected:

### 故障排查

#### skill 未触发

若 Claude 在预期时未使用你的 skill：

1. Check the description includes keywords users would naturally say
2. Verify the skill appears in `What skills are available?`
3. Try rephrasing your request to match the description more closely
4. Invoke it directly with `/skill-name` if the skill is user-invocable

1. 检查 description 是否包含用户自然会说出的关键词
2. 确认该 skill 出现在 `What skills are available?` 的回答中
3. 尝试改写请求，使其更贴近 description
4. 若该 skill 用户可调用，用 `/skill-name` 直接调用

If the frontmatter YAML is malformed, Claude Code loads the skill body with empty metadata, so `/skill-name` still works but Claude has no `description` to match against. Run with `--debug` to see the parse error.

若 frontmatter YAML 格式错误，Claude Code 会以空元数据加载 skill 正文，因此 `/skill-name` 仍可用，但 Claude 没有 `description` 可匹配。使用 `--debug` 运行可查看解析错误。

#### Skill triggers too often

If Claude uses your skill when you don't want it:

#### skill 触发过于频繁

若 Claude 在你不希望时使用了你的 skill：

1. Make the description more specific
2. Add `disable-model-invocation: true` if you only want manual invocation

1. 让 description 更具体
2. 若仅希望手动调用，添加 `disable-model-invocation: true`

#### Skill descriptions are cut short

Skill descriptions are loaded into context so Claude knows what's available. All skill names are always included, but if you have many skills, descriptions are shortened to fit the character budget, which can strip the keywords Claude needs to match your request. The budget scales at 1% of the model's context window. When it overflows, descriptions for the skills you invoke least are dropped first, so the skills you actually use keep their full text. Run `/doctor` to see how many skill descriptions are being shortened or dropped and which skills are affected.

#### skill description 被截断

skill 的 description 会载入上下文，让 Claude 知晓可用项。所有 skill 名称始终包含，但若 skill 很多，description 会被缩短以适配字符预算，这可能剥离 Claude 匹配请求所需的关键词。预算按模型上下文窗口的 1% 缩放。溢出时，最少调用的 skill 的 description 先被丢弃，使你实际使用的 skill 保留全文。运行 `/doctor` 可查看有多少 skill description 被缩短或丢弃，以及受影响的 skill。

As of v2.1.196, the Skills row in `/context` reports the size of the listing after the budget is applied, so it matches what the model receives. Earlier versions counted the full text of every description, so the row could show a value several times larger than the budget `/doctor` reports.

自 v2.1.196 起，`/context` 中的 Skills 行报告的是预算应用后的列表大小，与模型收到的内容一致。早期版本统计每条 description 的全文，因此该行可能显示比 `/doctor` 报告的预算大数倍的值。

To raise the budget, set the [`skillListingBudgetFraction`](/en/settings#available-settings) setting (e.g. `0.02` = 2%) or the `SLASH_COMMAND_TOOL_CHAR_BUDGET` environment variable to a fixed character count. To free budget for other skills, set low-priority entries to `"name-only"` in [`skillOverrides`](#override-skill-visibility-from-settings) so they list without a description. You can also trim the `description` and `when_to_use` text at the source: put the key use case first, since each entry's combined text is capped at 1,536 characters regardless of budget. The cap is configurable with [`skillListingMaxDescChars`](/en/settings#available-settings).

要提高预算，设置 [`skillListingBudgetFraction`](/en/settings#available-settings)（如 `0.02` = 2%）或 `SLASH_COMMAND_TOOL_CHAR_BUDGET` 环境变量为固定字符数。要为其它 skill 腾出预算，在 [`skillOverrides`](#override-skill-visibility-from-settings) 中将低优先级条目设为 `"name-only"`，使其仅列出名称不带 description。你也可从源头精简 `description` 和 `when_to_use` 文本：把关键用例放最前，因为每条合并文本上限为 1,536 字符（与预算无关）。该上限可用 [`skillListingMaxDescChars`](/en/settings#available-settings) 配置。

### Related resources

* **[Debug your configuration](/en/debug-your-config)**: diagnose why a skill isn't appearing or triggering
* **[Evaluating skill output quality](https://agentskills.io/skill-creation/evaluating-skills)**: the eval file format and iteration workflow on agentskills.io
* **[Skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)**: writing guidance that applies across Claude products
* **[Subagents](/en/sub-agents)**: delegate tasks to specialized agents
* **[Plugins](/en/plugins)**: package and distribute skills with other extensions
* **[Hooks](/en/hooks)**: automate workflows around tool events
* **[Memory](/en/memory)**: manage CLAUDE.md files for persistent context
* **[Commands](/en/commands)**: reference for built-in commands and bundled skills
* **[Permissions](/en/permissions)**: control tool and skill access
* **[Claude Tag skills](https://claude.com/docs/claude-tag/admins/skills-repo)**: project skills committed to a repo also load when that repo is used in a Claude Tag channel

### 相关资源

* **[调试你的配置](/en/debug-your-config)**：诊断 skill 为何不出现或不触发
* **[评估 skill 输出质量](https://agentskills.io/skill-creation/evaluating-skills)**：agentskills.io 上的 eval 文件格式与迭代工作流
* **[skill 编写最佳实践](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)**：适用于各 Claude 产品的编写指引
* **[Subagents](/en/sub-agents)**：将任务委派给专门的 agent
* **[Plugins](/en/plugins)**：打包并分发 skill 及其它扩展
* **[Hooks](/en/hooks)**：围绕工具事件自动化工作流
* **[Memory](/en/memory)**：管理 CLAUDE.md 文件以持久化上下文
* **[Commands](/en/commands)**：内置命令与内置 skill 参考
* **[Permissions](/en/permissions)**：控制工具与 skill 访问
* **[Claude Tag skills](https://claude.com/docs/claude-tag/admins/skills-repo)**：提交到仓库的项目 skill 在该仓库被用于 Claude Tag 频道时也会加载

---

## Claude Code - Subagents（子代理）

原文链接：https://code.claude.com/docs/en/sub-agents


### Create custom subagents

> Create and use specialized AI subagents in Claude Code for task-specific workflows and improved context management.

> 在 Claude Code 中创建并使用专门的 AI subagent（子代理），以处理特定任务的工作流（workflow）并改善上下文管理。

Subagents are specialized AI assistants that handle specific types of tasks. Use one when a side task would flood your main conversation with search results, logs, or file contents you won't reference again: the subagent does that work in its own context and returns only the summary. Define a custom subagent when you keep spawning the same kind of worker with the same instructions.

Subagent 是一种专门处理特定类型任务的 AI 助手。当某个附带任务会用搜索结果、日志或文件内容淹没你的主对话，而这些内容你之后不会再引用时，就使用 subagent：它在自己的上下文中完成工作，只返回摘要。当你反复用同样的指令去生成同一类工作者时，就定义一个自定义 subagent。

Each subagent runs in its own context window with a custom system prompt, specific tool access, and independent permissions. When Claude encounters a task that matches a subagent's description, it delegates to that subagent, which works independently and returns results. To see the context savings in practice, the [context window visualization](/en/context-window) walks through a session where a subagent handles research in its own separate window.

每个 subagent 都运行在自己独立的上下文窗口中，拥有自定义的 system prompt（系统提示词）、特定的工具访问权限和独立的权限。当 Claude 遇到与某个 subagent 描述匹配的任务时，它会委派给该 subagent，由其独立工作并返回结果。要直观了解上下文的节省效果，可参考 [上下文窗口可视化](/en/context-window)，它演示了一个 subagent 如何在单独的窗口中处理研究工作。

> Subagents work within a single session. To run many independent sessions in parallel and monitor them from one place, see [background agents](/en/agent-view). For sessions that communicate with each other, see [agent teams](/en/agent-teams).

> Subagent 在单个会话内工作。若要并行运行多个独立会话并集中监控，参见 [background agents（后台代理）](/en/agent-view)。若要实现会话间相互通信，参见 [agent teams（代理团队）](/en/agent-teams)。

Subagents help you:

Subagent 可帮助你：

* **Preserve context** by keeping exploration and implementation out of your main conversation
* **Enforce constraints** by limiting which tools a subagent can use
* **Reuse configurations** across projects with user-level subagents
* **Specialize behavior** with focused system prompts for specific domains
* **Control costs** by routing tasks to faster, cheaper models like Haiku

* **保留上下文** —— 把探索和实现工作挡在主对话之外
* **强制约束** —— 限制 subagent 能使用哪些工具
* **复用配置** —— 通过 user 级 subagent 在多个项目中复用
* **特化行为** —— 用聚焦特定领域的 system prompt 来定制行为
* **控制成本** —— 把任务路由到更快、更便宜的模型，例如 Haiku

Claude uses each subagent's description to decide when to delegate tasks. When you create a subagent, write a clear description so Claude knows when to use it.

Claude 依据每个 subagent 的 description 来决定何时委派任务。创建 subagent 时，写一段清晰的描述，好让 Claude 知道何时该用它。

Claude Code includes several built-in subagents such as Explore, Plan, and general-purpose. You can also create custom subagents to handle specific tasks.

Claude Code 内置了若干 subagent，例如 Explore、Plan 和 general-purpose。你也可以创建自定义 subagent 来处理特定任务。

### Built-in subagents

Claude Code includes built-in subagents that Claude automatically uses when appropriate. Each inherits the parent conversation's permissions with additional tool restrictions.

### 内置 subagent

Claude Code 内置了若干 subagent，Claude 会在合适时自动使用。每个内置 subagent 都继承父对话的权限，并附加工具限制。

Explore and Plan skip your CLAUDE.md files and the parent session's git status to keep research fast and inexpensive. Every other built-in and [custom subagent](#configure-subagents) loads both. For the full breakdown of what reaches a subagent, see [what loads at startup](#what-loads-at-startup).

Explore 和 Plan 会跳过你的 CLAUDE.md 文件以及父会话的 git status，以保持研究过程快速且低开销。其他所有内置 subagent 和 [自定义 subagent](#configure-subagents) 都会加载这两者。关于哪些内容会传入 subagent 的完整说明，参见 [启动时加载的内容](#what-loads-at-startup)。

**Explore** — A fast, read-only agent optimized for searching and analyzing codebases.

**Explore** —— 一个快速、只读的 agent（代理），针对代码库的搜索与分析做了优化。

* **Model**: inherits from the main conversation, capped at Opus on the Claude API, so Explore never runs on a more expensive model than the one you already chose for the session
* **Tools**: read-only tools; Write and Edit are denied
* **Purpose**: file discovery, code search, codebase exploration

* **模型**：继承主对话的模型，在 Claude API 上封顶为 Opus，因此 Explore 永远不会跑得比你会话所选模型更贵
* **工具**：只读工具；Write 和 Edit 被禁用
* **用途**：文件发现、代码搜索、代码库探索

As of v2.1.198, Explore inherits the main conversation's model instead of always running on Haiku. On the Claude API, the inherited model is capped at Opus: a main conversation on a higher tier runs Explore on Opus, and a main conversation on Sonnet or Haiku runs Explore on that same model. On any other provider, such as [Amazon Bedrock, Google Cloud's Agent Platform, Microsoft Foundry, or Claude Platform on AWS](/en/third-party-integrations), Explore inherits the main conversation's model directly.

自 v2.1.198 起，Explore 继承主对话的模型，而不再总是运行在 Haiku 上。在 Claude API 上，继承的模型封顶为 Opus：主对话处于更高档位时 Explore 跑在 Opus 上，主对话跑在 Sonnet 或 Haiku 时 Explore 也跑在同一模型上。在其他任何提供商上（例如 [Amazon Bedrock、Google Cloud 的 Agent Platform、Microsoft Foundry 或 AWS 上的 Claude Platform](/en/third-party-integrations)），Explore 直接继承主对话的模型。

A [user or project subagent](#choose-the-subagent-scope) named `Explore` overrides the built-in and keeps its own `model` field, so define one with `model: haiku` to keep exploration on a lower-cost model.

一个名为 `Explore` 的 [user 或 project subagent](#choose-the-subagent-scope) 会覆盖内置项并保留自己的 `model` 字段，因此可以用 `model: haiku` 定义一个，把探索工作留在更便宜的模型上。

Claude delegates to Explore when it needs to search or understand a codebase without making changes. This keeps exploration results out of your main conversation context.

当 Claude 需要搜索或理解代码库而不做改动时，会委派给 Explore。这样可以把探索结果挡在主对话上下文之外。

When invoking Explore, Claude specifies a thoroughness level: **quick** for targeted lookups, **medium** for balanced exploration, or **very thorough** for comprehensive analysis.

调用 Explore 时，Claude 会指定一个彻底程度：**quick** 用于定向查找，**medium** 用于平衡型探索，**very thorough** 用于全面分析。

**Plan** — A research agent used during [plan mode](/en/permission-modes#analyze-before-you-edit-with-plan-mode) to gather context before presenting a plan.

**Plan** —— 一个在 [plan 模式](/en/permission-modes#analyze-before-you-edit-with-plan-mode) 下使用的研究型 agent，用于在给出方案前收集上下文。

* **Model**: inherits from the main conversation
* **Tools**: read-only tools; Write and Edit are denied
* **Purpose**: codebase research for planning

* **模型**：继承主对话
* **工具**：只读工具；Write 和 Edit 被禁用
* **用途**：为规划做代码库研究

When you're in plan mode and Claude needs to understand your codebase, it delegates research to the Plan subagent so that exploration output stays in a separate context window while the main conversation remains read-only.

当你处于 plan 模式且 Claude 需要理解你的代码库时，它会把研究工作委派给 Plan subagent，使探索输出留在单独的上下文窗口中，而主对话保持只读。

**General-purpose** — A capable agent for complex, multi-step tasks that require both exploration and action.

**General-purpose** —— 一个能力较强的 agent，用于既需要探索又需要行动的复杂多步骤任务。

* **Model**: inherits from the main conversation
* **Tools**: all tools
* **Purpose**: complex research, multi-step operations, code modifications

* **模型**：继承主对话
* **工具**：全部工具
* **用途**：复杂研究、多步骤操作、代码修改

Claude delegates to general-purpose when the task requires both exploration and modification, complex reasoning to interpret results, or multiple dependent steps.

当任务既需要探索又需要修改、需要复杂推理来解读结果，或涉及多个相互依赖的步骤时，Claude 会委派给 general-purpose。

**Other helper agents** — Claude Code includes additional helper agents for specific tasks. These are typically invoked automatically, so you don't need to use them directly.

**其他辅助 agent** —— Claude Code 还包含针对特定任务的辅助 agent，通常会被自动调用，你无需直接使用。

| Agent             | Model  | When Claude uses it                                      |
| :---------------- | :----- | :------------------------------------------------------- |
| statusline-setup  | Sonnet | When you run `/statusline` to configure your status line |
| claude-code-guide | Haiku  | When you ask questions about Claude Code features        |

| Agent             | 模型   | Claude 何时使用                                          |
| :---------------- | :----- | :------------------------------------------------------- |
| statusline-setup  | Sonnet | 运行 `/statusline` 配置状态栏时                          |
| claude-code-guide | Haiku  | 询问 Claude Code 功能相关问题时                          |

Built-in subagents are registered by default in interactive sessions. To restrict them:

内置 subagent 在交互式会话中默认注册。若要限制它们：

* To block a specific built-in type, add it to `permissions.deny` as shown in [Disable specific subagents](#disable-specific-subagents).
* To prevent Claude from delegating to any subagent, deny the `Agent` tool itself with [`permissions.deny`](/en/permissions#tool-specific-permission-rules).
* To remove only the built-in `Explore` and `Plan` subagents, set [`CLAUDE_CODE_DISABLE_EXPLORE_PLAN_AGENTS=1`](/en/env-vars). Claude reads and explores files directly instead of delegating to them. Requires Claude Code v2.1.198 or later.
* In [non-interactive mode](/en/headless) and the [Agent SDK](/en/agent-sdk/overview), set [`CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS=1`](/en/env-vars) to remove all built-in types and supply only your own.

* 要屏蔽某个具体的内置类型，把它加入 `permissions.deny`，参见 [禁用特定 subagent](#disable-specific-subagents)。
* 要阻止 Claude 委派给任何 subagent，用 [`permissions.deny`](/en/permissions#tool-specific-permission-rules) 禁用 `Agent` 工具本身。
* 要仅移除内置的 `Explore` 和 `Plan` subagent，设置 [`CLAUDE_CODE_DISABLE_EXPLORE_PLAN_AGENTS=1`](/en/env-vars)。Claude 会直接读取并探索文件，而不是委派给它们。需要 Claude Code v2.1.198 或更高版本。
* 在 [非交互模式](/en/headless) 和 [Agent SDK](/en/agent-sdk/overview) 中，设置 [`CLAUDE_AGENT_SDK_DISABLE_BUILTIN_AGENTS=1`](/en/env-vars) 可移除所有内置类型，只提供你自己的。

Beyond these built-in subagents, you can create your own with custom prompts, tool restrictions, permission modes, hooks, and skills. The following sections show how to get started and customize subagents.

除这些内置 subagent 外，你还可以用自定义 prompt、工具限制、权限模式、hook（钩子）和 skill（技能）创建自己的 subagent。下面几节介绍如何上手并自定义 subagent。

### Quickstart: create your first subagent

Subagents are Markdown files with YAML frontmatter. To create one, ask Claude to write it for you, or [write the file yourself](#write-subagent-files).

### 快速开始：创建你的第一个 subagent

Subagent 是带有 YAML frontmatter 的 Markdown 文件。要创建一个，可以让 Claude 帮你写，也可以 [自己写文件](#write-subagent-files)。

As of v2.1.198, the `/agents` command no longer opens the interactive creation wizard; running it prints a reminder to ask Claude or edit `.claude/agents/` directly. Subagent files, frontmatter fields, and the `.claude/agents/` and `~/.claude/agents/` locations are unchanged; only the terminal wizard is removed.

自 v2.1.198 起，`/agents` 命令不再打开交互式创建向导；运行它只会打印一条提醒，让你去问 Claude 或直接编辑 `.claude/agents/`。Subagent 文件、frontmatter 字段以及 `.claude/agents/` 和 `~/.claude/agents/` 这两个位置都没有变化，仅移除了终端向导。

This walkthrough creates a user-level subagent that reviews code and suggests improvements.

本演练将创建一个 user 级 subagent，用于审查代码并提出改进建议。

**Step 1: Ask Claude to create the subagent** — In Claude Code, describe the subagent you want and where to save it:

**第 1 步：让 Claude 创建 subagent** —— 在 Claude Code 中，描述你想要的 subagent 以及保存位置：

```text wrap theme={null}
Create a personal code-improver subagent in ~/.claude/agents/ that scans
files and suggests improvements for readability, performance, and best
practices. It should explain each issue, show the current code, and
provide an improved version. Make it read-only and have it use Sonnet.
```

Claude writes the file with a `name`, a `description`, a `tools` list, a `model`, and a system prompt.

Claude 会写出带有 `name`、`description`、`tools` 列表、`model` 和 system prompt 的文件。

**Step 2: Review the file** — Open `~/.claude/agents/code-improver.md` and confirm the frontmatter matches what you asked for. The result looks like this:

**第 2 步：检查文件** —— 打开 `~/.claude/agents/code-improver.md`，确认 frontmatter 与你的要求一致。结果如下：

```markdown theme={null}
---
name: code-improver
description: Scans files and suggests improvements for readability, performance, and best practices. Use after writing or modifying code.
tools: Read, Grep, Glob
model: sonnet
---

You are a code improvement specialist. For each issue you find, explain
the problem, show the current code, and provide an improved version.
```

Because the file lives in `~/.claude/agents/`, the subagent is available in every project on your machine. To scope it to one project instead, move it to that project's `.claude/agents/` directory. [Choose the subagent scope](#choose-the-subagent-scope) compares the two.

由于文件位于 `~/.claude/agents/`，该 subagent 在你机器上的所有项目中都可用。若要限定到单个项目，把它移到该项目的 `.claude/agents/` 目录即可。[选择 subagent 作用域](#choose-the-subagent-scope) 对两者做了对比。

**Step 3: Try it out** — Ask Claude to delegate to the new subagent:

**第 3 步：试用** —— 让 Claude 委派给新 subagent：

```text wrap theme={null}
Use the code-improver agent to suggest improvements in this project
```

Claude delegates to your new subagent, which scans the codebase and returns improvement suggestions.

Claude 会委派给你的新 subagent，由它扫描代码库并返回改进建议。

If Claude can't find the new subagent, restart Claude Code and try again. This happens only when `~/.claude/agents/` didn't exist before the session started, because a running session doesn't detect a newly created `agents` directory.

如果 Claude 找不到新 subagent，重启 Claude Code 后重试。这种情况只会在会话开始前 `~/.claude/agents/` 不存在时发生，因为运行中的会话检测不到新建的 `agents` 目录。

You now have a subagent you can use in any project on your machine to analyze codebases and suggest improvements.

现在你有了一个可在任意项目中使用的 subagent，用于分析代码库并提出改进建议。

You can also write subagent files by hand, define them via CLI flags, or distribute them through plugins. The following sections cover all configuration options.

你也可以手写 subagent 文件、通过 CLI 标志定义，或通过插件分发。后续章节会介绍全部配置选项。

> On Claude Code v2.1.197 and earlier, `/agents` opens an interactive wizard with a **Running** tab that lists live subagents and a **Library** tab for creating, editing, and deleting them.

> 在 Claude Code v2.1.197 及更早版本中，`/agents` 会打开一个交互式向导，其中 **Running** 标签页列出运行中的 subagent，**Library** 标签页用于创建、编辑和删除它们。

### Configure subagents

A subagent's file location determines who it's available to, and its frontmatter determines what it can do. This section covers where subagent files live and every field they support.

### 配置 subagent

Subagent 的文件位置决定它对谁可用，frontmatter 决定它能做什么。本节介绍 subagent 文件存放在哪里以及支持的全部字段。

#### Choose the subagent scope

Store subagent files in different locations depending on scope. When multiple subagents share the same name, Claude Code uses the one from the higher-priority location.

#### 选择 subagent 作用域

根据作用域把 subagent 文件存放在不同位置。当多个 subagent 同名时，Claude Code 使用优先级更高位置上的那个。

| Location                     | Scope                   | Priority    | How to create                                 |
| :--------------------------- | :---------------------- | :---------- | :-------------------------------------------- |
| Managed settings             | Organization-wide       | 1 (highest) | Deployed via [managed settings](/en/settings) |
| `--agents` CLI flag          | Current session         | 2           | Pass JSON when launching Claude Code          |
| `.claude/agents/`            | Current project         | 3           | Ask Claude, or create the file manually       |
| `~/.claude/agents/`          | All your projects       | 4           | Ask Claude, or create the file manually       |
| Plugin's `agents/` directory | Where plugin is enabled | 5 (lowest)  | Installed with [plugins](/en/plugins)         |

| 位置                         | 作用域                  | 优先级      | 创建方式                                      |
| :--------------------------- | :---------------------- | :---------- | :-------------------------------------------- |
| Managed settings             | 全组织                  | 1（最高）   | 通过 [managed settings](/en/settings) 部署    |
| `--agents` CLI 标志          | 当前会话                | 2           | 启动 Claude Code 时传入 JSON                  |
| `.claude/agents/`            | 当前项目                | 3           | 让 Claude 写，或手动创建文件                  |
| `~/.claude/agents/`          | 你的所有项目            | 4           | 让 Claude 写，或手动创建文件                  |
| 插件的 `agents/` 目录        | 启用插件处              | 5（最低）   | 随 [插件](/en/plugins) 安装                   |

**Project subagents** (`.claude/agents/`) are ideal for subagents specific to a codebase. Check them into version control so your team can use and improve them collaboratively.

**Project subagent**（`.claude/agents/`）适合针对特定代码库的 subagent。把它们纳入版本控制，团队就能共同使用和改进。

Project subagents are discovered by walking up from the current working directory, so every `.claude/agents/` between there and the repository root is scanned. As of v2.1.178, when more than one of these nested directories defines the same `name`, Claude Code uses the definition closest to the working directory.

Project subagent 通过从当前工作目录向上遍历来发现，因此从工作目录到仓库根目录之间的每个 `.claude/agents/` 都会被扫描。自 v2.1.178 起，当多个嵌套目录定义了同一个 `name` 时，Claude Code 使用离工作目录最近的那个定义。

Directories added with `--add-dir` are also scanned: a `.claude/agents/` folder inside an added directory loads alongside project subagents. See [Additional directories](/en/permissions#additional-directories-grant-file-access-not-configuration) for which other configuration types load from `--add-dir`. To share subagents across projects without `--add-dir`, use `~/.claude/agents/` or a [plugin](/en/plugins).

用 `--add-dir` 添加的目录也会被扫描：被添加目录中的 `.claude/agents/` 文件夹会和 project subagent 一起加载。关于还有哪些配置类型会从 `--add-dir` 加载，参见 [附加目录](/en/permissions#additional-directories-grant-file-access-not-configuration)。要在不带 `--add-dir` 的情况下跨项目共享 subagent，使用 `~/.claude/agents/` 或某个 [插件](/en/plugins)。

**User subagents** (`~/.claude/agents/`) are personal subagents available in all your projects.

**User subagent**（`~/.claude/agents/`）是个人 subagent，在你所有项目中可用。

Claude Code scans `.claude/agents/` and `~/.claude/agents/` recursively, so you can organize definitions into subfolders such as `agents/review/` or `agents/research/`. The subdirectory path doesn't affect how a subagent is identified or invoked, because identity comes only from the `name` frontmatter field.

Claude Code 会递归扫描 `.claude/agents/` 和 `~/.claude/agents/`，因此可以把定义组织进子文件夹，例如 `agents/review/` 或 `agents/research/`。子目录路径不影响 subagent 的标识或调用方式，因为标识仅来自 `name` frontmatter 字段。

Keep `name` values unique across the whole tree: if two files within one scope declare the same name, Claude Code loads only one of them. As of v2.1.196, running `/doctor` reports same-scope duplicate agent names and shows which definition is active.

整个目录树内 `name` 值要保持唯一：如果同一作用域内两个文件声明了相同名称，Claude Code 只加载其中一个。自 v2.1.196 起，运行 `/doctor` 会报告同作用域内重复的 agent 名称，并显示当前生效的是哪个定义。

Plugin `agents/` directories are also scanned recursively. Unlike project and user scopes, a subfolder inside a plugin's `agents/` directory becomes part of the [scoped identifier](#invoke-subagents-explicitly): a file at `agents/review/security.md` in plugin `my-plugin` registers as `my-plugin:review:security`.

插件的 `agents/` 目录也会被递归扫描。与 project 和 user 作用域不同，插件 `agents/` 目录内的子文件夹会成为 [带作用域标识符](#invoke-subagents-explicitly) 的一部分：插件 `my-plugin` 中 `agents/review/security.md` 文件会注册为 `my-plugin:review:security`。

**CLI-defined subagents** are passed as JSON when launching Claude Code. They exist only for that session and aren't saved to disk, making them useful for quick testing or automation scripts. You can define multiple subagents in a single `--agents` call:

**CLI 定义的 subagent** 在启动 Claude Code 时以 JSON 形式传入。它们只存在于当前会话、不写入磁盘，适合快速测试或自动化脚本。可在一次 `--agents` 调用中定义多个 subagent：

```bash theme={null}
claude --agents '{
  "code-reviewer": {
    "description": "Expert code reviewer. Use proactively after code changes.",
    "prompt": "You are a senior code reviewer. Focus on code quality, security, and best practices.",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  },
  "debugger": {
    "description": "Debugging specialist for errors and test failures.",
    "prompt": "You are an expert debugger. Analyze errors, identify root causes, and provide fixes."
  }
}'
```

```powershell theme={null}
claude --agents @'
{
  "code-reviewer": {
    "description": "Expert code reviewer. Use proactively after code changes.",
    "prompt": "You are a senior code reviewer. Focus on code quality, security, and best practices.",
    "tools": ["Read", "Grep", "Glob", "Bash"],
    "model": "sonnet"
  },
  "debugger": {
    "description": "Debugging specialist for errors and test failures.",
    "prompt": "You are an expert debugger. Analyze errors, identify root causes, and provide fixes."
  }
}
'@
```

The `--agents` flag accepts JSON with the same [frontmatter](#supported-frontmatter-fields) fields as file-based subagents: `description`, `prompt`, `tools`, `disallowedTools`, `model`, `permissionMode`, `mcpServers`, `hooks`, `maxTurns`, `skills`, `initialPrompt`, `memory`, `effort`, `background`, `isolation`, and `color`. Use `prompt` for the system prompt, equivalent to the markdown body in file-based subagents.

`--agents` 标志接受的 JSON 字段与文件式 subagent 的 [frontmatter](#supported-frontmatter-fields) 字段相同：`description`、`prompt`、`tools`、`disallowedTools`、`model`、`permissionMode`、`mcpServers`、`hooks`、`maxTurns`、`skills`、`initialPrompt`、`memory`、`effort`、`background`、`isolation` 和 `color`。用 `prompt` 表示 system prompt，等价于文件式 subagent 中的 markdown 正文。

**Managed subagents** are deployed by organization administrators. Place markdown files in `.claude/agents/` inside the [managed settings directory](/en/settings#settings-files), using the same frontmatter format as project and user subagents. Managed definitions take precedence over project and user subagents with the same name.

**Managed subagent** 由组织管理员部署。把 markdown 文件放在 [managed settings 目录](/en/settings#settings-files) 下的 `.claude/agents/` 中，使用与 project 和 user subagent 相同的 frontmatter 格式。Managed 定义优先于同名的 project 和 user subagent。

**Plugin subagents** come from [plugins](/en/plugins) you've installed. They load alongside your custom subagents and appear in the @-mention typeahead under their scoped name. See the [plugin components reference](/en/plugins-reference#agents) for details on creating plugin subagents.

**Plugin subagent** 来自你已安装的 [插件](/en/plugins)。它们与你的自定义 subagent 一起加载，并在 @-提及 的候选列表中以带作用域的名称出现。创建插件 subagent 的细节参见 [插件组件参考](/en/plugins-reference#agents)。

> For security reasons, plugin subagents don't support the `hooks`, `mcpServers`, or `permissionMode` frontmatter fields. These fields are ignored when loading agents from a plugin. If you need them, copy the agent file into `.claude/agents/` or `~/.claude/agents/`. You can also add rules to [`permissions.allow`](/en/settings#permission-settings) in `settings.json` or `settings.local.json`, but these rules apply to the entire session, not only the plugin subagent.

> 出于安全原因，plugin subagent 不支持 `hooks`、`mcpServers` 或 `permissionMode` frontmatter 字段。从插件加载 agent 时这些字段会被忽略。如果你需要它们，把 agent 文件复制到 `.claude/agents/` 或 `~/.claude/agents/` 中。你也可以在 `settings.json` 或 `settings.local.json` 中向 [`permissions.allow`](/en/settings#permission-settings) 添加规则，但这些规则作用于整个会话，而非仅作用于该 plugin subagent。

Subagent definitions from any of these scopes are also available to [agent teams](/en/agent-teams#use-subagent-definitions-for-teammates): when spawning a teammate, you can reference a subagent type and the teammate uses its `tools` and `model`, with the definition's body appended to the teammate's system prompt as additional instructions. See [agent teams](/en/agent-teams#use-subagent-definitions-for-teammates) for which frontmatter fields apply on that path.

以上任意作用域的 subagent 定义也可供 [agent teams](/en/agent-teams#use-subagent-definitions-for-teammates) 使用：生成一个 teammate 时，你可以引用某个 subagent 类型，teammate 会使用它的 `tools` 和 `model`，并把该定义的正文作为附加指令追加到 teammate 的 system prompt 中。该路径下哪些 frontmatter 字段生效，参见 [agent teams](/en/agent-teams#use-subagent-definitions-for-teammates)。

#### Write subagent files

Subagent files use YAML frontmatter for configuration, followed by the system prompt in Markdown:

#### 编写 subagent 文件

Subagent 文件用 YAML frontmatter 做配置，其后用 Markdown 写 system prompt：

> Claude Code watches `~/.claude/agents/` and `.claude/agents/`. When you add or edit a subagent file on disk, or ask Claude to write one for you, Claude Code detects the change within a few seconds and the next delegation uses the updated definition, with no restart needed.

> Claude Code 会监视 `~/.claude/agents/` 和 `.claude/agents/`。当你在磁盘上新增或编辑 subagent 文件，或让 Claude 替你写一个时，Claude Code 会在几秒内检测到变更，下次委派即使用更新后的定义，无需重启。

Two cases still need a restart:

* The watcher covers only directories that existed when the session started, so after creating a scope's first agent file in a new `agents` directory, restart to load it.
* Sessions started with `--disable-slash-commands` don't watch these directories at all.

两种情况仍需重启：

* 监视器只覆盖会话启动时已存在的目录，因此在新 `agents` 目录中为某作用域创建第一个 agent 文件后，需重启才能加载。
* 以 `--disable-slash-commands` 启动的会话完全不监视这些目录。

```markdown theme={null}
---
name: code-reviewer
description: Reviews code for quality and best practices
tools: Read, Glob, Grep
model: sonnet
---

You are a code reviewer. When invoked, analyze the code and provide
specific, actionable feedback on quality, security, and best practices.
```

The frontmatter defines the subagent's metadata and configuration. The body becomes the system prompt that guides the subagent's behavior. Subagents receive only this system prompt plus basic environment details like the working directory, not the full Claude Code system prompt.

frontmatter 定义 subagent 的元数据和配置。正文成为 system prompt，用于引导 subagent 的行为。Subagent 只接收这个 system prompt 加上工作目录等基础环境信息，不接收完整的 Claude Code system prompt。

A subagent starts in the main conversation's current working directory. Within a subagent, `cd` commands don't persist between Bash or PowerShell tool calls and don't affect the main conversation's working directory. To give the subagent an isolated copy of the repository instead, set [`isolation: worktree`](#supported-frontmatter-fields).

Subagent 以主对话的当前工作目录为起始目录。在 subagent 内部，`cd` 命令不会在 Bash 或 PowerShell 工具调用之间保留，也不会影响主对话的工作目录。若要给 subagent 一份隔离的仓库副本，可设置 [`isolation: worktree`](#supported-frontmatter-fields)。

#### Supported frontmatter fields

The following fields can be used in the YAML frontmatter. Only `name` and `description` are required.

#### 支持的 frontmatter 字段

以下字段可用于 YAML frontmatter。只有 `name` 和 `description` 是必填项。

| Field             | Required | Description                                                                                                                                                                                                                                                                                                                              |
| :---------------- | :------- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`            | Yes      | Unique identifier using lowercase letters and hyphens. [Hooks](/en/hooks#subagentstart) receive this value as `agent_type`. The filename doesn't have to match                                                                                                                                                                           |
| `description`     | Yes      | When Claude should delegate to this subagent                                                                                                                                                                                                                                                                                             |
| `tools`           | No       | [Tools](#available-tools) the subagent can use. Inherits all tools if omitted. To preload Skills into context, use the `skills` field rather than listing `Skill` here                                                                                                                                                                   |
| `disallowedTools` | No       | Tools to deny, removed from inherited or specified list                                                                                                                                                                                                                                                                                  |
| `model`           | No       | [Model](#choose-a-model) to use: `sonnet`, `opus`, `haiku`, `fable`, a full model ID (for example, `claude-opus-4-8`), or `inherit`. Defaults to `inherit`                                                                                                                                                                               |
| `permissionMode`  | No       | [Permission mode](#permission-modes): `default`, `acceptEdits`, `auto`, `dontAsk`, `bypassPermissions`, `plan`, or `manual` as an alias for `default`. The `manual` alias requires Claude Code v2.1.200 or later. Ignored for [plugin subagents](#choose-the-subagent-scope)                                 |
| `maxTurns`        | No       | Maximum number of agentic turns before the subagent stops                                                                                                                                                                                                                                                                                |
| `skills`          | No       | [Skills](/en/skills) to preload into the subagent's context at startup. The full skill content is injected, not only the description. Subagents can still invoke unlisted project, user, and plugin skills through the Skill tool                                                                                                        |
| `mcpServers`      | No       | [MCP servers](/en/mcp) available to this subagent. Each entry is either a server name referencing an already-configured server (e.g., `"slack"`) or an inline definition with the server name as key and a full [MCP server config](/en/mcp#installing-mcp-servers) as value. Ignored for [plugin subagents](#choose-the-subagent-scope) |
| `hooks`           | No       | [Lifecycle hooks](#define-hooks-for-subagents) scoped to this subagent. Ignored for [plugin subagents](#choose-the-subagent-scope)                                                                                                                                                                                                       |
| `memory`          | No       | [Persistent memory scope](#enable-persistent-memory): `user`, `project`, or `local`. Enables cross-session learning                                                                                                                                                                                                                      |
| `background`      | No       | Set to `true` to always run this subagent as a [background task](#run-subagents-in-foreground-or-background), even when Claude needs its result right away. When unset, Claude chooses, and as of v2.1.198 it runs subagents in the background by default                                                    |
| `effort`          | No       | Effort level when this subagent is active. Overrides the session effort level. Default: inherits from session. Options: `low`, `medium`, `high`, `xhigh`, `max`; available levels depend on the model                                                                                                                                    |
| `isolation`       | No       | Set to `worktree` to run the subagent in a temporary [git worktree](/en/worktrees), giving it an isolated copy of the repository branched by default from your [default branch](/en/worktrees#choose-the-base-branch) rather than the parent session's `HEAD`. The worktree is automatically cleaned up if the subagent makes no changes |
| `color`           | No       | Display color for the subagent in the task list and transcript. Accepts `red`, `blue`, `green`, `yellow`, `purple`, `orange`, `pink`, or `cyan`                                                                                                                                                                                          |
| `initialPrompt`   | No       | Auto-submitted as the first user turn when this agent runs as the main session agent (via `--agent` or the `agent` setting). [Commands](/en/commands) and [skills](/en/skills) are processed. Prepended to any user-provided prompt                                                                                                      |

| 字段              | 必填 | 说明                                                                                                                                                                                                                                                                                                                                     |
| :---------------- | :--- | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`            | 是   | 唯一标识符，使用小写字母和连字符。[Hooks](/en/hooks#subagentstart) 以 `agent_type` 形式接收该值。文件名不必与之一致                                                                                                                                                                                                                       |
| `description`     | 是   | Claude 何时应委派给该 subagent                                                                                                                                                                                                                                                                                                            |
| `tools`           | 否   | subagent 可用的 [工具](#available-tools)。省略时继承全部工具。要把 Skill 预加载进上下文，请用 `skills` 字段，而不是在这里列出 `Skill`                                                                                                                                                                                                     |
| `disallowedTools` | 否   | 要禁用的工具，从继承或指定的列表中移除                                                                                                                                                                                                                                                                                                     |
| `model`           | 否   | 使用的 [模型](#choose-a-model)：`sonnet`、`opus`、`haiku`、`fable`、完整模型 ID（例如 `claude-opus-4-8`）或 `inherit`。默认 `inherit`                                                                                                                                                                                                     |
| `permissionMode`  | 否   | [权限模式](#permission-modes)：`default`、`acceptEdits`、`auto`、`dontAsk`、`bypassPermissions`、`plan`，或作为 `default` 别名的 `manual`。`manual` 别名需 Claude Code v2.1.200 或更高版本。对 [plugin subagent](#choose-the-subagent-scope) 忽略                                                          |
| `maxTurns`        | 否   | subagent 停止前的最大 agentic 轮数                                                                                                                                                                                                                                                                                                        |
| `skills`          | 否   | 启动时预加载进 subagent 上下文的 [Skills](/en/skills)。注入的是完整 skill 内容，而非仅描述。Subagent 仍可通过 Skill 工具调用未列出的 project、user 和 plugin skill                                                                                                                                                                          |
| `mcpServers`      | 否   | 该 subagent 可用的 [MCP 服务器](/en/mcp)。每项既可以是一个引用已配置服务器的名称（如 `"slack"`），也可以是内联定义，以服务器名为键、完整 [MCP server 配置](/en/mcp#installing-mcp-servers) 为值。对 [plugin subagent](#choose-the-subagent-scope) 忽略                                                                                     |
| `hooks`           | 否   | 作用于该 subagent 的 [生命周期 hooks](#define-hooks-for-subagents)。对 [plugin subagent](#choose-the-subagent-scope) 忽略                                                                                                                                                                                                                |
| `memory`          | 否   | [持久化 memory 作用域](#enable-persistent-memory)：`user`、`project` 或 `local`。启用跨会话学习                                                                                                                                                                                                                                          |
| `background`      | 否   | 设为 `true` 则始终把该 subagent 作为 [后台任务](#run-subagents-in-foreground-or-background) 运行，即使 Claude 立即需要其结果。未设置时由 Claude 决定，且自 v2.1.198 起默认在后台运行 subagent                                                                                                                                             |
| `effort`          | 否   | 该 subagent 激活时的 effort 等级。覆盖会话的 effort 等级。默认：继承自会话。选项：`low`、`medium`、`high`、`xhigh`、`max`；可用等级取决于模型                                                                                                                                                                                                |
| `isolation`       | 否   | 设为 `worktree` 可在临时 [git worktree](/en/worktrees) 中运行 subagent，给它一份隔离的仓库副本，默认从你的 [默认分支](/en/worktrees#choose-the-base-branch) 分出，而非父会话的 `HEAD`。若 subagent 未做改动，worktree 会自动清理                                                                                                                                 |
| `color`           | 否   | subagent 在任务列表和记录中的显示颜色。接受 `red`、`blue`、`green`、`yellow`、`purple`、`orange`、`pink` 或 `cyan`                                                                                                                                                                                                                       |
| `initialPrompt`   | 否   | 当该 agent 作为主会话 agent 运行时（通过 `--agent` 或 `agent` 设置），自动作为首个 user 轮提交。会处理 [命令](/en/commands) 和 [skills](/en/skills)。前置到任何用户提供的 prompt 之前                                                                                                                                                  |

### Choose a model

The `model` field controls which [AI model](/en/model-config) the subagent uses:

### 选择模型

`model` 字段控制 subagent 使用哪个 [AI 模型](/en/model-config)：

* **Model alias**: use one of the available aliases: `sonnet`, `opus`, `haiku`, or `fable`
* **Full model ID**: use a full model ID such as `claude-opus-4-8` or `claude-sonnet-5`. Accepts the same values as the `--model` flag
* **inherit**: use the same model as the main conversation
* **Omitted**: defaults to `inherit` and uses the same model as the main conversation

* **模型别名**：使用可用别名之一：`sonnet`、`opus`、`haiku` 或 `fable`
* **完整模型 ID**：使用完整模型 ID，例如 `claude-opus-4-8` 或 `claude-sonnet-5`。接受与 `--model` 标志相同的值
* **inherit**：使用与主对话相同的模型
* **省略**：默认为 `inherit`，使用与主对话相同的模型

When Claude invokes a subagent, it can also pass a `model` parameter for that specific invocation. Claude Code resolves the subagent's model in this order:

当 Claude 调用 subagent 时，也可以为该次调用传入 `model` 参数。Claude Code 按以下顺序解析 subagent 的模型：

1. The [`CLAUDE_CODE_SUBAGENT_MODEL`](/en/model-config#environment-variables) environment variable, when set to a model alias or model ID
2. The per-invocation `model` parameter
3. The subagent definition's `model` frontmatter
4. The main conversation's model

1. [`CLAUDE_CODE_SUBAGENT_MODEL`](/en/model-config#environment-variables) 环境变量（设为模型别名或模型 ID 时）
2. 单次调用的 `model` 参数
3. subagent 定义的 `model` frontmatter
4. 主对话的模型

As of v2.1.196, setting `CLAUDE_CODE_SUBAGENT_MODEL` to `inherit` is the same as leaving it unset: resolution continues with the per-invocation `model` parameter, then the frontmatter. In earlier versions, `inherit` forced subagents onto the main conversation's model and ignored both of those sources.

自 v2.1.196 起，把 `CLAUDE_CODE_SUBAGENT_MODEL` 设为 `inherit` 与不设置等效：解析会继续到单次调用的 `model` 参数，再到 frontmatter。在更早版本中，`inherit` 会强制 subagent 使用主对话的模型，并忽略这两个来源。

Claude Code checks the environment variable, per-invocation parameter, and frontmatter values against your organization's [`availableModels`](/en/model-config#restrict-model-selection) allowlist. It skips a value that resolves to an excluded model and runs the subagent on the inherited model instead.

Claude Code 会把环境变量、单次调用参数和 frontmatter 的值对照你组织的 [`availableModels`](/en/model-config#restrict-model-selection) 白名单检查。若某值解析到被排除的模型，则跳过该值，改用继承的模型运行 subagent。

As of v2.1.198, subagents also inherit the main conversation's [extended thinking](/en/model-config#extended-thinking) configuration: if thinking is on in your session, it's on for the subagent, and if it's off, it stays off. There is no per-subagent thinking setting. Before v2.1.198, subagents ran with extended thinking disabled regardless of the main conversation's setting.

自 v2.1.198 起，subagent 也继承主对话的 [extended thinking](/en/model-config#extended-thinking) 配置：会话中开启则 subagent 开启，关闭则保持关闭。没有针对单个 subagent 的 thinking 设置。在 v2.1.198 之前，无论主对话如何设置，subagent 都在禁用 extended thinking 的状态下运行。

### Control subagent capabilities

You can control what subagents can do through tool access, permission modes, and conditional rules.

### 控制 subagent 能力

你可以通过工具访问、权限模式和条件规则来控制 subagent 能做什么。

#### Available tools

Subagents inherit the [internal tools](/en/tools-reference) and MCP tools available in the main conversation by default. The following tools depend on the main conversation's UI or session state and aren't available to subagents, even when listed in the `tools` field:

#### 可用工具

Subagent 默认继承主对话中可用的 [内部工具](/en/tools-reference) 和 MCP 工具。以下工具依赖主对话的 UI 或会话状态，即使列在 `tools` 字段中也无法供 subagent 使用：

* `AskUserQuestion`
* `EnterPlanMode`
* `ExitPlanMode`, unless the subagent's [`permissionMode`](#permission-modes) is `plan`
* `ScheduleWakeup`
* `WaitForMcpServers`

* `AskUserQuestion`
* `EnterPlanMode`
* `ExitPlanMode`（除非 subagent 的 [`permissionMode`](#permission-modes) 为 `plan`）
* `ScheduleWakeup`
* `WaitForMcpServers`

To restrict tools, use the `tools` field as an allowlist or the `disallowedTools` field as a denylist. This example uses `tools` to allow only Read, Grep, Glob, and Bash. The subagent can't edit files, write files, or use any MCP tools:

要限制工具，可把 `tools` 字段用作白名单，或把 `disallowedTools` 字段用作黑名单。下面的例子用 `tools` 仅允许 Read、Grep、Glob 和 Bash。该 subagent 无法编辑文件、写入文件或使用任何 MCP 工具：

```yaml theme={null}
---
name: safe-researcher
description: Research agent with restricted capabilities
tools: Read, Grep, Glob, Bash
---
```

This example uses `disallowedTools` to inherit every tool from the main conversation except Write and Edit. The subagent keeps Bash, MCP tools, and everything else:

下面的例子用 `disallowedTools` 继承主对话的全部工具，但 Write 和 Edit 除外。该 subagent 保留 Bash、MCP 工具及其他所有工具：

```yaml theme={null}
---
name: no-writes
description: Inherits every tool except file writes
disallowedTools: Write, Edit
---
```

If both are set, `disallowedTools` is applied first, then `tools` is resolved against the remaining pool. A tool listed in both is removed.

两者都设置时，先应用 `disallowedTools`，再用 `tools` 对剩余工具池做解析。同时出现在两者中的工具会被移除。

Both fields accept MCP server-level patterns in addition to exact tool names: `mcp__<server>` or `mcp__<server>__*` grants or removes every tool from the named server. In `disallowedTools`, `mcp__*` also removes every MCP tool from any server. This example removes every tool from the `github` MCP server while keeping tools from other servers and every built-in tool:

两个字段除接受精确工具名外，还接受 MCP 服务器级模式：`mcp__<server>` 或 `mcp__<server>__*` 会授予或移除该命名服务器的全部工具。在 `disallowedTools` 中，`mcp__*` 还会移除任意服务器的全部 MCP 工具。下面的例子移除 `github` MCP 服务器的全部工具，同时保留其他服务器的工具和所有内置工具：

```yaml theme={null}
---
name: local-only
description: Inherits every tool except those from the github MCP server
disallowedTools: mcp__github
---
```

#### Restrict which subagents can be spawned

When an agent runs as the main thread with `claude --agent`, it can spawn subagents using the Agent tool. To restrict which subagent types it can spawn, use `Agent(agent_type)` syntax in the `tools` field.

#### 限制可生成的 subagent

当 agent 以 `claude --agent` 作为主线程运行时，可用 Agent 工具生成 subagent。要限制它能生成哪些 subagent 类型，在 `tools` 字段中使用 `Agent(agent_type)` 语法。

> In version 2.1.63, the Task tool was renamed to Agent. Existing `Task(...)` references in settings and agent definitions still work as aliases.

> 在 2.1.63 版本中，Task 工具更名为 Agent。设置和 agent 定义中已有的 `Task(...)` 引用仍作为别名有效。

```yaml theme={null}
---
name: coordinator
description: Coordinates work across specialized agents
tools: Agent(worker, researcher), Read, Bash
---
```

This is an allowlist: only the `worker` and `researcher` subagents can be spawned. If the agent tries to spawn any other type, the request fails and the agent sees only the allowed types in its prompt. To block specific agents while allowing all others, use [`permissions.deny`](#disable-specific-subagents) instead.

这是一个白名单：只能生成 `worker` 和 `researcher` subagent。如果 agent 尝试生成其他类型，请求会失败，并且 agent 在其 prompt 中只能看到允许的类型。要在放行其余 agent 的同时屏蔽特定 agent，改用 [`permissions.deny`](#disable-specific-subagents)。

To allow spawning any subagent without restrictions, use `Agent` without parentheses:

要无限制地允许生成任意 subagent，使用不带括号的 `Agent`：

```yaml theme={null}
tools: Agent, Read, Bash
```

If `Agent` is omitted from the `tools` list entirely, the agent can't spawn any subagents.

如果 `tools` 列表中完全没有 `Agent`，则该 agent 无法生成任何 subagent。

The `Agent(agent_type)` allowlist syntax applies only to an agent running as the main thread with `claude --agent`. In a subagent definition, listing `Agent` in `tools` lets that subagent [spawn nested subagents](#spawn-nested-subagents), but any type list inside the parentheses is ignored.

`Agent(agent_type)` 白名单语法仅适用于以 `claude --agent` 作为主线程运行的 agent。在 subagent 定义中，把 `Agent` 列入 `tools` 可让该 subagent [生成嵌套 subagent](#spawn-nested-subagents)，但括号内的任何类型列表都会被忽略。

#### Scope MCP servers to a subagent

Use the `mcpServers` field to give a subagent access to [MCP](/en/mcp) servers that aren't available in the main conversation. Inline servers defined here are connected when the subagent starts and disconnected when it finishes. String references share the parent session's connection.

#### 为 subagent 限定 MCP 服务器

用 `mcpServers` 字段给 subagent 访问主对话中不可用的 [MCP](/en/mcp) 服务器。此处定义的内联服务器在 subagent 启动时连接、结束时断开。字符串引用则共享父会话的连接。

> The `mcpServers` field applies in both contexts where an agent file can run:
>
> * As a subagent, spawned through the Agent tool or an @-mention
> * As the main session, launched with [`--agent`](#invoke-subagents-explicitly) or the `agent` setting
>
> When the agent is the main session, inline server definitions connect at startup alongside servers from [`.mcp.json`](/en/mcp) and settings files.

> `mcpServers` 字段适用于 agent 文件可运行的两种上下文：
>
> * 作为 subagent，通过 Agent 工具或 @-提及 生成
> * 作为主会话，通过 [`--agent`](#invoke-subagents-explicitly) 或 `agent` 设置启动
>
> 当 agent 作为主会话时，内联服务器定义会在启动时与 [`.mcp.json`](/en/mcp) 和设置文件中的服务器一同连接。

Each entry in the list is either an inline server definition or a string referencing an MCP server already configured in your session:

列表中的每一项要么是内联服务器定义，要么是引用会话中已配置 MCP 服务器的字符串：

```yaml theme={null}
---
name: browser-tester
description: Tests features in a real browser using Playwright
mcpServers:
  # Inline definition: scoped to this subagent only
  - playwright:
      type: stdio
      command: npx
      args: ["-y", "@playwright/mcp@latest"]
  # Reference by name: reuses an already-configured server
  - github
---

Use the Playwright tools to navigate, screenshot, and interact with pages.
```

Inline definitions use the same schema as `.mcp.json` server entries, keyed by the server name, and support the `stdio`, `http`, `sse`, and `ws` types.

内联定义使用与 `.mcp.json` 服务器条目相同的 schema，以服务器名为键，支持 `stdio`、`http`、`sse` 和 `ws` 类型。

To keep an MCP server out of the main conversation entirely and avoid its tool descriptions consuming context there, define it inline here rather than in `.mcp.json`. The subagent gets the tools; the parent conversation doesn't.

要让某个 MCP 服务器完全不出现在主对话中、避免其工具描述在那里消耗上下文，就在此处内联定义它，而不是放进 `.mcp.json`。Subagent 拿到工具；父对话则没有。

As of v2.1.153, the MCP restrictions that apply to the main session also cover servers declared in subagent frontmatter:

自 v2.1.153 起，适用于主会话的 MCP 限制也覆盖在 subagent frontmatter 中声明的服务器：

* [`--strict-mcp-config`](/en/cli-reference) and [`--bare`](/en/cli-reference)
* [Enterprise managed MCP configuration](/en/managed-mcp)
* [`allowedMcpServers` and `deniedMcpServers` policies](/en/managed-mcp#policy-based-control-with-allowlists-and-denylists)

* [`--strict-mcp-config`](/en/cli-reference) 和 [`--bare`](/en/cli-reference)
* [企业 managed MCP 配置](/en/managed-mcp)
* [`allowedMcpServers` 和 `deniedMcpServers` 策略](/en/managed-mcp#policy-based-control-with-allowlists-and-denylists)

When one of these blocks a server, Claude Code skips it and shows a warning naming the blocked servers.

当其中某项屏蔽了某个服务器时，Claude Code 会跳过它并显示一条警告，列出被屏蔽的服务器。

Managed-settings restrictions apply to every subagent regardless of how it is defined. `--strict-mcp-config` doesn't filter servers you pass inline via `--agents` or the SDK `agents` option, since those are explicit caller input.

Managed-settings 的限制适用于每个 subagent，无论它如何定义。`--strict-mcp-config` 不会过滤你通过 `--agents` 或 SDK 的 `agents` 选项内联传入的服务器，因为那些是调用方的显式输入。

#### Permission modes

The `permissionMode` field controls how the subagent handles permission prompts. Subagents inherit the permission context from the main conversation and can override the mode, except when the parent mode takes precedence as described below.

#### 权限模式

`permissionMode` 字段控制 subagent 如何处理权限提示。Subagent 继承主对话的权限上下文并可覆盖该模式，但在下文所述父模式优先的情况下除外。

| Mode                | Behavior                                                                                                                                    |
| :------------------ | :------------------------------------------------------------------------------------------------------------------------------------------ |
| `default`           | Standard permission checking with prompts                                                                                                   |
| `acceptEdits`       | Auto-accept file edits and common filesystem commands for paths in the working directory or `additionalDirectories`                         |
| `auto`              | [Auto mode](/en/permission-modes#eliminate-prompts-with-auto-mode): a background classifier reviews commands and protected-directory writes |
| `dontAsk`           | Auto-deny permission prompts (explicitly allowed tools still work)                                                                          |
| `bypassPermissions` | Skip permission prompts                                                                                                                     |
| `plan`              | Plan mode (read-only exploration)                                                                                                           |

| 模式               | 行为                                                                                                                                        |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| `default`          | 标准权限检查，带提示                                                                                                                         |
| `acceptEdits`      | 自动接受工作目录或 `additionalDirectories` 中路径的文件编辑和常见文件系统命令                                                                |
| `auto`             | [auto 模式](/en/permission-modes#eliminate-prompts-with-auto-mode)：后台分类器审查命令和受保护目录的写入                                    |
| `dontAsk`          | 自动拒绝权限提示（显式允许的工具仍可用）                                                                                                     |
| `bypassPermissions`| 跳过权限提示                                                                                                                                 |
| `plan`             | plan 模式（只读探索）                                                                                                                        |

> Use `bypassPermissions` with caution. It skips permission prompts, allowing the subagent to execute operations without approval, including writes to `.git`, `.config/git`, `.claude`, `.vscode`, `.idea`, `.husky`, `.cargo`, `.devcontainer`, `.yarn`, and `.mvn`. Explicit [`ask` rules](/en/permissions#manage-permissions) and root and home directory removals such as `rm -rf /` still prompt. See [permission modes](/en/permission-modes#skip-all-checks-with-bypasspermissions-mode) for details.

> 谨慎使用 `bypassPermissions`。它会跳过权限提示，允许 subagent 未经批准就执行操作，包括写入 `.git`、`.config/git`、`.claude`、`.vscode`、`.idea`、`.husky`、`.cargo`、`.devcontainer`、`.yarn` 和 `.mvn`。显式的 [`ask` 规则](/en/permissions#manage-permissions) 以及根目录和家目录的删除操作（如 `rm -rf /`）仍会提示。详见 [权限模式](/en/permission-modes#skip-all-checks-with-bypasspermissions-mode)。

If the parent uses `bypassPermissions` or `acceptEdits`, this takes precedence and can't be overridden. If the parent uses [auto mode](/en/permission-modes#eliminate-prompts-with-auto-mode), the subagent inherits auto mode and any `permissionMode` in its frontmatter is ignored: the classifier evaluates the subagent's tool calls with the same block and allow rules as the parent session.

若父级使用 `bypassPermissions` 或 `acceptEdits`，则该模式优先且无法被覆盖。若父级使用 [auto 模式](/en/permission-modes#eliminate-prompts-with-auto-mode)，则 subagent 继承 auto 模式，其 frontmatter 中的任何 `permissionMode` 都会被忽略：分类器以与父会话相同的拦截和放行规则评估 subagent 的工具调用。

#### Preload skills into subagents

Use the `skills` field to inject skill content into a subagent's context at startup. This gives the subagent domain knowledge without requiring it to discover and load skills during execution.

#### 把 skill 预加载进 subagent

用 `skills` 字段在启动时把 skill 内容注入 subagent 的上下文。这样 subagent 无需在执行过程中发现并加载 skill 就拥有领域知识。

```yaml theme={null}
---
name: api-developer
description: Implement API endpoints following team conventions
skills:
  - api-conventions
  - error-handling-patterns
---

Implement API endpoints. Follow the conventions and patterns from the preloaded skills.
```

The full content of each listed skill is injected into the subagent's context at startup. This field controls which skills are preloaded, not which skills the subagent can access: without it, the subagent can still discover and invoke project, user, and plugin skills through the Skill tool during execution. To prevent a subagent from invoking skills entirely, omit `Skill` from the [`tools`](#available-tools) list or add it to `disallowedTools`.

每个所列 skill 的完整内容都会在启动时注入 subagent 的上下文。该字段控制的是预加载哪些 skill，而非 subagent 能访问哪些 skill：没有它，subagent 在执行过程中仍可通过 Skill 工具发现并调用 project、user 和 plugin skill。要完全阻止 subagent 调用 skill，从 [`tools`](#available-tools) 列表中省略 `Skill`，或把它加入 `disallowedTools`。

You can't preload skills that set [`disable-model-invocation: true`](/en/skills#control-who-invokes-a-skill), since preloading draws from the same set of skills Claude can invoke. If a listed skill is missing or disabled, Claude Code skips it and logs a warning to the debug log.

你无法预加载设置了 [`disable-model-invocation: true`](/en/skills#control-who-invokes-a-skill) 的 skill，因为预加载取自 Claude 可调用的同一 skill 集合。若所列 skill 缺失或被禁用，Claude Code 会跳过它并在调试日志中记录一条警告。

> This is the inverse of [running a skill in a subagent](/en/skills#run-skills-in-a-subagent). With `skills` in a subagent, the subagent controls the system prompt and loads skill content. With `context: fork` in a skill, the skill content is injected into the agent you specify. Both use the same underlying system.

> 这与 [在 subagent 中运行 skill](/en/skills#run-skills-in-a-subagent) 恰好相反。在 subagent 中用 `skills` 时，subagent 控制 system prompt 并加载 skill 内容。在 skill 中用 `context: fork` 时，skill 内容被注入你指定的 agent。两者使用同一底层系统。

#### Enable persistent memory

The `memory` field gives the subagent a persistent directory that survives across conversations. The subagent uses this directory to build up knowledge over time, such as codebase patterns, debugging insights, and architectural decisions.

#### 启用持久化 memory

`memory` 字段给 subagent 一个跨会话存活的持久化目录。Subagent 用该目录随时间积累知识，例如代码库模式、调试心得和架构决策。

```yaml theme={null}
---
name: code-reviewer
description: Reviews code for quality and best practices
memory: user
---

You are a code reviewer. As you review code, update your agent memory with
patterns, conventions, and recurring issues you discover.
```

Choose a scope based on how broadly the memory should apply:

根据 memory 应适用的范围选择作用域：

| Scope     | Location                                      | Use when                                                                                   |
| :-------- | :-------------------------------------------- | :----------------------------------------------------------------------------------------- |
| `user`    | `~/.claude/agent-memory/<name-of-agent>/`     | the subagent should remember learnings across all projects                                 |
| `project` | `.claude/agent-memory/<name-of-agent>/`       | the subagent's knowledge is project-specific and shareable via version control             |
| `local`   | `.claude/agent-memory-local/<name-of-agent>/` | the subagent's knowledge is project-specific but shouldn't be checked into version control |

| 作用域    | 位置                                          | 适用场景                                                                                   |
| :-------- | :-------------------------------------------- | :----------------------------------------------------------------------------------------- |
| `user`    | `~/.claude/agent-memory/<name-of-agent>/`     | subagent 应跨所有项目记住所学                                                              |
| `project` | `.claude/agent-memory/<name-of-agent>/`       | subagent 的知识是项目特定的，且可通过版本控制共享                                          |
| `local`   | `.claude/agent-memory-local/<name-of-agent>/` | subagent 的知识是项目特定的，但不应纳入版本控制                                            |

When memory is enabled:

启用 memory 后：

* The subagent's system prompt includes instructions for reading and writing to the memory directory.
* The subagent's system prompt also includes the first 200 lines or 25KB of `MEMORY.md` in the memory directory, whichever comes first, with instructions to curate `MEMORY.md` if it exceeds that limit.
* Read, Write, and Edit tools are automatically enabled so the subagent can manage its memory files.

* subagent 的 system prompt 中包含读写 memory 目录的指令。
* subagent 的 system prompt 还包含 memory 目录中 `MEMORY.md` 的前 200 行或 25KB（以先到者为准），并附带在超出该限制时整理 `MEMORY.md` 的指令。
* Read、Write 和 Edit 工具会被自动启用，以便 subagent 管理其 memory 文件。

##### Persistent memory tips

* `project` is the recommended default scope. It makes subagent knowledge shareable via version control.
* Ask the subagent to consult its memory before starting work: "Review this PR, and check your memory for patterns you've seen before."

##### 持久化 memory 提示

* `project` 是推荐的默认作用域。它让 subagent 的知识可通过版本控制共享。
* 在开始工作前让 subagent 查阅其 memory：「审查这个 PR，并查看你的 memory 中是否见过相关模式。」


---

## Claude Code - GitHub Actions

原文链接：https://code.claude.com/docs/en/github-actions


### Claude Code GitHub Actions

> Learn about integrating Claude Code into your development workflow with Claude Code GitHub Actions

Claude Code GitHub Actions brings AI-powered automation to your GitHub workflow. With a simple `@claude` mention in any PR or issue, Claude can analyze your code, create pull requests, implement features, and fix bugs - all while following your project's standards. For automatic reviews posted on every PR without a trigger, see [GitHub Code Review](/en/code-review).

> 了解如何通过 Claude Code GitHub Actions 将 Claude Code 集成到你的开发工作流中。

Claude Code GitHub Actions 为你的 GitHub 工作流带来 AI 驱动的自动化能力。只需在任意 PR (pull request，拉取请求) 或 issue (议题) 中 `@claude` 一下，Claude 就能分析你的代码、创建 PR、实现功能并修复 Bug —— 同时严格遵循你项目的规范。如需了解在每个 PR 上自动触发而无需手动 @ 的代码评审，请参阅 [GitHub Code Review](/en/code-review)。

<Note>
  Claude Code GitHub Actions is built on top of the [Claude Agent SDK](/en/agent-sdk/overview), which enables programmatic integration of Claude Code into your applications. You can use the SDK to build custom automation workflows beyond GitHub Actions.
</Note>

<Note>
  Claude Code GitHub Actions 基于 [Claude Agent SDK](/en/agent-sdk/overview) 构建，后者支持以编程方式将 Claude Code 集成到你的应用中。你可以使用该 SDK 构建超越 GitHub Actions 的自定义自动化工作流。
</Note>

### Why use Claude Code GitHub Actions?

* **Instant PR creation**: Describe what you need, and Claude creates a complete PR with all necessary changes
* **Automated code implementation**: Turn issues into working code with a single command
* **Follows your standards**: Claude respects your `CLAUDE.md` guidelines and existing code patterns
* **Simple setup**: Get started in minutes with our installer and API key
* **Secure by default**: Your code stays on Github's runners

### 为什么要使用 Claude Code GitHub Actions？

* **即时创建 PR**：只需描述你的需求，Claude 就会创建一个包含所有必要改动的完整 PR
* **自动化代码实现**：一条命令即可将 issue 转化为可运行代码
* **遵循你的规范**：Claude 会尊重 `CLAUDE.md` 指南和现有代码模式
* **配置简单**：通过安装脚本和 API key，几分钟即可上手
* **默认安全**：你的代码始终运行在 GitHub 的 runner 上

### What can Claude do?

Claude Code provides a powerful GitHub Action that transforms how you work with code:

### Claude 能做什么？

Claude Code 提供了一个强大的 GitHub Action，能够改变你与代码协作的方式：

### Claude Code Action

This GitHub Action allows you to run Claude Code within your GitHub Actions workflows. You can use this to build any custom workflow on top of Claude Code.

[View repository →](https://github.com/anthropics/claude-code-action)

### Claude Code Action

该 GitHub Action 允许你在 GitHub Actions 工作流中运行 Claude Code。你可以基于此构建任意自定义工作流。

[查看仓库 →](https://github.com/anthropics/claude-code-action)

### Setup

### Quick setup

Run `/install-github-app` in the Claude Code terminal to set up the integration interactively. The command installs the Claude GitHub App on your repository and then walks you through adding the GitHub Actions workflows and the API key secret.

After the GitHub App is installed, the command asks whether to continue with GitHub Actions setup. In Claude Code v2.1.187 and later you can choose **Skip for now** to stop with only the App installed and return to the workflow and secret steps by running `/install-github-app` again. Earlier versions proceed straight to workflow selection.

### 安装配置

### 快速安装

在 Claude Code 终端中运行 `/install-github-app`，以交互式方式完成集成配置。该命令会在你的仓库上安装 Claude GitHub App，随后引导你添加 GitHub Actions 工作流和 API key secret。

GitHub App 安装完成后，命令会询问是否继续进行 GitHub Actions 配置。在 Claude Code v2.1.187 及更高版本中，你可以选择 **Skip for now** 仅安装 App，之后再次运行 `/install-github-app` 即可回到工作流和 secret 步骤。更早的版本则会直接进入工作流选择。

<Note>
  * You must be a repository admin to install the GitHub app and add secrets
  * The GitHub app will request read & write permissions for Contents, Issues, and Pull requests
  * This quickstart method is only available for direct Claude API users. If
    you're using Amazon Bedrock or Google Cloud's Agent Platform, see the [Using
    with Amazon Bedrock and Google Cloud](#using-with-amazon-bedrock-and-google-cloud)
    section.
</Note>

<Note>
  * 你必须是仓库管理员才能安装 GitHub App 并添加 secrets
  * 该 GitHub App 会请求对 Contents、Issues 和 Pull requests 的读写权限
  * 该快速上手方式仅适用于直连 Claude API 的用户。如果你使用的是 Amazon Bedrock 或 Google Cloud 的 Agent Platform，请参阅 [与 Amazon Bedrock 和 Google Cloud 配合使用](#using-with-amazon-bedrock-and-google-cloud) 一节。
</Note>

### Manual setup

If the `/install-github-app` command fails or you prefer manual setup, please follow these manual setup instructions:

1. **Install the Claude GitHub app** to your repository: [https://github.com/apps/claude](https://github.com/apps/claude)

   The Claude GitHub app requires the following repository permissions:

   * **Contents**: Read & write (to modify repository files)
   * **Issues**: Read & write (to respond to issues)
   * **Pull requests**: Read & write (to create PRs and push changes)

   For more details on security and permissions, see the [security documentation](https://github.com/anthropics/claude-code-action/blob/main/docs/security.md).
2. **Add ANTHROPIC\_API\_KEY** to your repository secrets ([Learn how to use secrets in GitHub Actions](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions))
3. **Copy the workflow file** from [examples/claude.yml](https://github.com/anthropics/claude-code-action/blob/main/examples/claude.yml) into your repository's `.github/workflows/`

### 手动安装

如果 `/install-github-app` 命令执行失败，或者你更倾向于手动配置，请按以下步骤操作：

1. **将 Claude GitHub App 安装到你的仓库**：[https://github.com/apps/claude](https://github.com/apps/claude)

   Claude GitHub App 需要以下仓库权限：

   * **Contents**：读写权限（用于修改仓库文件）
   * **Issues**：读写权限（用于响应 issue）
   * **Pull requests**：读写权限（用于创建 PR 并推送改动）

   关于安全与权限的更多细节，请参阅 [安全文档](https://github.com/anthropics/claude-code-action/blob/main/docs/security.md)。
2. **将 `ANTHROPIC_API_KEY` 添加到仓库 secrets 中**（[了解如何在 GitHub Actions 中使用 secrets](https://docs.github.com/en/actions/security-guides/using-secrets-in-github-actions)）
3. **复制工作流文件** [examples/claude.yml](https://github.com/anthropics/claude-code-action/blob/main/examples/claude.yml) 到你仓库的 `.github/workflows/` 目录下

<Tip>
  After completing either the quickstart or manual setup, test the action by tagging `@claude` in an issue or PR comment.
</Tip>

<Tip>
  完成快速上手或手动配置后，在 issue 或 PR 评论中 @ 一下 `@claude` 即可测试该 Action 是否生效。
</Tip>

### Upgrading from Beta

<Warning>
  Claude Code GitHub Actions v1.0 introduces breaking changes that require updating your workflow files in order to upgrade to v1.0 from the beta version.
</Warning>

If you're currently using the beta version of Claude Code GitHub Actions, we recommend that you update your workflows to use the GA version. The new version simplifies configuration while adding powerful new features like automatic mode detection.

### 从 Beta 版本升级

<Warning>
  Claude Code GitHub Actions v1.0 引入了破坏性变更，从 Beta 版本升级到 v1.0 时必须更新你的工作流文件。
</Warning>

如果你当前使用的是 Claude Code GitHub Actions 的 Beta 版本，建议你将工作流更新到 GA 版本。新版本简化了配置，同时新增了自动模式检测等强大特性。

### Essential changes

All beta users must make these changes to their workflow files in order to upgrade:

1. **Update the action version**: Change `@beta` to `@v1`
2. **Remove mode configuration**: Delete `mode: "tag"` or `mode: "agent"` (now auto-detected)
3. **Update prompt inputs**: Replace `direct_prompt` with `prompt`
4. **Move CLI options**: Convert `max_turns`, `model`, `custom_instructions`, etc. to `claude_args`

### 必要改动

所有 Beta 用户都必须对工作流文件做如下改动才能完成升级：

1. **更新 Action 版本**：将 `@beta` 改为 `@v1`
2. **移除模式配置**：删除 `mode: "tag"` 或 `mode: "agent"`（现已自动检测）
3. **更新 prompt 输入**：将 `direct_prompt` 替换为 `prompt`
4. **迁移 CLI 选项**：将 `max_turns`、`model`、`custom_instructions` 等迁移到 `claude_args`

### Breaking Changes Reference

| Old Beta Input        | New v1.0 Input                        |
| --------------------- | ------------------------------------- |
| `mode`                | *(Removed - auto-detected)*           |
| `direct_prompt`       | `prompt`                              |
| `override_prompt`     | `prompt` with GitHub variables        |
| `custom_instructions` | `claude_args: --append-system-prompt` |
| `max_turns`           | `claude_args: --max-turns`            |
| `model`               | `claude_args: --model`                |
| `allowed_tools`       | `claude_args: --allowedTools`         |
| `disallowed_tools`    | `claude_args: --disallowedTools`      |
| `claude_env`          | `settings` JSON format                |

### 破坏性变更对照表

| 旧版 Beta 输入        | 新版 v1.0 输入                        |
| --------------------- | ------------------------------------- |
| `mode`                | *（已移除 — 自动检测）*               |
| `direct_prompt`       | `prompt`                              |
| `override_prompt`     | `prompt`（搭配 GitHub 变量）          |
| `custom_instructions` | `claude_args: --append-system-prompt` |
| `max_turns`           | `claude_args: --max-turns`            |
| `model`               | `claude_args: --model`                |
| `allowed_tools`       | `claude_args: --allowedTools`         |
| `disallowed_tools`    | `claude_args: --disallowedTools`      |
| `claude_env`          | `settings` 的 JSON 格式               |

### Before and After Example

**Beta version:**

```yaml theme={null}
- uses: anthropics/claude-code-action@beta
  with:
    mode: "tag"
    direct_prompt: "Review this PR for security issues"
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    custom_instructions: "Follow our coding standards"
    max_turns: "10"
    model: "claude-sonnet-5"
```

**GA version (v1.0):**

```yaml theme={null}
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "Review this PR for security issues"
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    claude_args: |
      --append-system-prompt "Follow our coding standards"
      --max-turns 10
      --model claude-sonnet-5
```

### 升级前后示例

**Beta 版本：**

```yaml theme={null}
- uses: anthropics/claude-code-action@beta
  with:
    mode: "tag"
    direct_prompt: "Review this PR for security issues"
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    custom_instructions: "Follow our coding standards"
    max_turns: "10"
    model: "claude-sonnet-5"
```

**GA 版本（v1.0）：**

```yaml theme={null}
- uses: anthropics/claude-code-action@v1
  with:
    prompt: "Review this PR for security issues"
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    claude_args: |
      --append-system-prompt "Follow our coding standards"
      --max-turns 10
      --model claude-sonnet-5
```

<Tip>
  The action now automatically detects whether to run in interactive mode (responds to `@claude` mentions) or automation mode (runs immediately with a prompt) based on your configuration.
</Tip>

<Tip>
  该 Action 现在会根据你的配置自动判断运行模式：是交互模式（响应 `@claude` 提及），还是自动化模式（根据 prompt 立即运行）。
</Tip>

### Example use cases

Claude Code GitHub Actions can help you with a variety of tasks. The [examples directory](https://github.com/anthropics/claude-code-action/tree/main/examples) contains ready-to-use workflows for different scenarios.

### 示例用例

Claude Code GitHub Actions 可以帮你完成多种任务。[examples 目录](https://github.com/anthropics/claude-code-action/tree/main/examples) 中提供了面向不同场景的即用型工作流。

### Basic workflow

```yaml theme={null}
name: Claude Code
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
jobs:
  claude:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          # Responds to @claude mentions in comments
```

### 基础工作流

```yaml theme={null}
name: Claude Code
on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
jobs:
  claude:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          # 响应评论中的 @claude 提及
```

### Using skills

The `prompt` input accepts a [skill](/en/skills) invocation as well as plain text:

* For a skill in your repository's `.claude/skills/` directory, run `actions/checkout` before the action step and pass `/skill-name`.
* For a skill packaged in a plugin, install the plugin with the `plugin_marketplaces` and `plugins` inputs and pass the namespaced `/plugin-name:skill-name`.

The following workflow installs the `code-review` plugin and runs its skill on each new or updated pull request:

### 使用 skills

`prompt` 输入既支持纯文本，也支持调用 [skill](/en/skills)（技能）：

* 对于位于仓库 `.claude/skills/` 目录下的 skill，请在 action 步骤之前运行 `actions/checkout`，并传入 `/skill-name`。
* 对于打包在 plugin 中的 skill，请使用 `plugin_marketplaces` 和 `plugins` 输入安装该 plugin，并传入带命名空间的 `/plugin-name:skill-name`。

以下工作流会安装 `code-review` plugin，并在每个新建或更新的 PR 上运行其 skill：

```yaml theme={null}
name: Code Review
on:
  pull_request:
    types: [opened, synchronize]
jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          plugin_marketplaces: "https://github.com/anthropics/claude-code.git"
          plugins: "code-review@claude-code-plugins"
          prompt: "/code-review:code-review ${{ github.repository }}/pull/${{ github.event.pull_request.number }}"
```

### Custom automation with prompts

```yaml theme={null}
name: Daily Report
on:
  schedule:
    - cron: "0 9 * * *"
jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: "Generate a summary of yesterday's commits and open issues"
          claude_args: "--model opus"
```

### 通过 prompt 实现自定义自动化

```yaml theme={null}
name: Daily Report
on:
  schedule:
    - cron: "0 9 * * *"
jobs:
  report:
    runs-on: ubuntu-latest
    steps:
      - uses: anthropics/claude-code-action@v1
        with:
          anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
          prompt: "Generate a summary of yesterday's commits and open issues"
          claude_args: "--model opus"
```

### Common use cases

In issue or PR comments:

```text wrap theme={null}
@claude implement this feature based on the issue description
@claude how should I implement user authentication for this endpoint?
@claude fix the TypeError in the user dashboard component
```

Claude will automatically analyze the context and respond appropriately.

### 常见用例

在 issue 或 PR 评论中：

```text wrap theme={null}
@claude implement this feature based on the issue description
@claude how should I implement user authentication for this endpoint?
@claude fix the TypeError in the user dashboard component
```

Claude 会自动分析上下文并作出恰当响应。

### Best practices

### CLAUDE.md configuration

Create a `CLAUDE.md` file in your repository root to define code style guidelines, review criteria, project-specific rules, and preferred patterns. This file guides Claude's understanding of your project standards.

### 最佳实践

### CLAUDE.md 配置

在仓库根目录创建一个 `CLAUDE.md` 文件，用于定义代码风格规范、评审标准、项目专属规则以及推荐的模式。该文件会指导 Claude 理解你的项目标准。

### Security considerations

<Warning>Never commit API keys directly to your repository.</Warning>

For comprehensive security guidance including permissions, authentication, and best practices, see the [Claude Code Action security documentation](https://github.com/anthropics/claude-code-action/blob/main/docs/security.md).

Always use GitHub Secrets for API keys:

* Add your API key as a repository secret named `ANTHROPIC_API_KEY`
* Reference it in workflows: `anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}`
* Limit action permissions to only what's necessary
* Review Claude's suggestions before merging

Always use GitHub Secrets (for example, `${{ secrets.ANTHROPIC_API_KEY }}`) rather than hardcoding API keys directly in your workflow files.

### 安全注意事项

<Warning>切勿将 API key 直接提交到仓库中。</Warning>

关于权限、身份验证和最佳实践等更全面的安全指引，请参阅 [Claude Code Action 安全文档](https://github.com/anthropics/claude-code-action/blob/main/docs/security.md)。

始终使用 GitHub Secrets 来存储 API key：

* 将你的 API key 添加为名为 `ANTHROPIC_API_KEY` 的仓库 secret
* 在工作流中引用它：`anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}`
* 将 Action 权限限制到最低必要范围
* 合并前务必评审 Claude 的建议

请始终使用 GitHub Secrets（例如 `${{ secrets.ANTHROPIC_API_KEY }}`），而不要将 API key 直接硬编码在工作流文件中。

### Optimizing performance

Use issue templates to provide context, keep your `CLAUDE.md` concise and focused, and configure appropriate timeouts for your workflows.

### 性能优化

使用 issue 模板来提供上下文，保持 `CLAUDE.md` 简洁聚焦，并为工作流配置合理的超时时间。

### CI costs

When using Claude Code GitHub Actions, be aware of the associated costs:

**GitHub Actions costs:**

* Claude Code runs on GitHub-hosted runners, which consume your GitHub Actions minutes
* See [GitHub's billing documentation](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions) for detailed pricing and minute limits

**API costs:**

* Each Claude interaction consumes API tokens based on the length of prompts and responses
* Token usage varies by task complexity and codebase size
* See [Claude's pricing page](https://claude.com/platform/api) for current token rates

### CI 成本

使用 Claude Code GitHub Actions 时，请注意相关成本：

**GitHub Actions 成本：**

* Claude Code 运行在 GitHub 托管的 runner 上，会消耗你的 GitHub Actions 分钟数
* 详细的定价和分钟数限额请参阅 [GitHub 计费文档](https://docs.github.com/en/billing/managing-billing-for-your-products/managing-billing-for-github-actions/about-billing-for-github-actions)

**API 成本：**

* 每次 Claude 交互都会根据 prompt 和响应的长度消耗 API token
* token 用量因任务复杂度和代码库规模而异
* 当前 token 费率请参阅 [Claude 定价页面](https://claude.com/platform/api)

**Cost optimization tips:**

* Use specific `@claude` commands to reduce unnecessary API calls
* Configure appropriate `--max-turns` in `claude_args` to prevent excessive iterations
* Set workflow-level timeouts to avoid runaway jobs
* Consider using GitHub's concurrency controls to limit parallel runs

**成本优化建议：**

* 使用具体的 `@claude` 命令以减少不必要的 API 调用
* 在 `claude_args` 中配置合理的 `--max-turns` 以避免过多迭代
* 在工作流级别设置超时，防止任务失控
* 可考虑使用 GitHub 的并发控制来限制并行运行数量

### Configuration examples

The Claude Code Action v1 simplifies configuration with unified parameters:

```yaml theme={null}
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    prompt: "Your instructions here" # Optional
    claude_args: "--max-turns 5" # Optional CLI arguments
```

Key features:

* **Unified prompt interface** - Use `prompt` for all instructions
* **Skills** - Invoke installed [skills](/en/skills) directly from the prompt
* **CLI passthrough** - Any Claude Code CLI argument via `claude_args`
* **Flexible triggers** - Works with any GitHub event

Visit the [examples directory](https://github.com/anthropics/claude-code-action/tree/main/examples) for complete workflow files.

### 配置示例

Claude Code Action v1 通过统一参数简化了配置：

```yaml theme={null}
- uses: anthropics/claude-code-action@v1
  with:
    anthropic_api_key: ${{ secrets.ANTHROPIC_API_KEY }}
    prompt: "Your instructions here" # 可选
    claude_args: "--max-turns 5" # 可选的 CLI 参数
```

主要特性：

* **统一的 prompt 接口** — 所有指令都通过 `prompt` 传入
* **Skills** — 可直接在 prompt 中调用已安装的 [skills](/en/skills)
* **CLI 透传** — 通过 `claude_args` 传入任意 Claude Code CLI 参数
* **灵活的触发器** — 兼容任意 GitHub 事件

完整的工作流文件请访问 [examples 目录](https://github.com/anthropics/claude-code-action/tree/main/examples)。

<Tip>
  When responding to issue or PR comments, Claude automatically responds to @claude mentions. For other events, use the `prompt` parameter to provide instructions.
</Tip>

<Tip>
  在响应 issue 或 PR 评论时，Claude 会自动响应 `@claude` 提及。对于其他事件，请使用 `prompt` 参数提供指令。
</Tip>

### Using with Amazon Bedrock and Google Cloud

For enterprise environments, you can use Claude Code GitHub Actions with your own cloud infrastructure. This approach gives you control over data residency and billing while maintaining the same functionality.

### 与 Amazon Bedrock 和 Google Cloud 配合使用

在企业环境中，你可以将 Claude Code GitHub Actions 与自有云基础设施配合使用。这种方式让你在保持同样功能的同时，掌控数据驻留与计费。

### Prerequisites

Before setting up Claude Code GitHub Actions with cloud providers, you need:

#### For Google Cloud's Agent Platform:

1. A Google Cloud Project with Google Cloud's Agent Platform enabled
2. Workload Identity Federation configured for GitHub Actions
3. A service account with the required permissions
4. A GitHub App (recommended) or use the default GITHUB\_TOKEN

#### For Amazon Bedrock:

1. An AWS account with Amazon Bedrock enabled
2. GitHub OIDC Identity Provider configured in AWS
3. An IAM role with Amazon Bedrock permissions
4. A GitHub App (recommended) or use the default GITHUB\_TOKEN

### 前置条件

在配置 Claude Code GitHub Actions 与云服务商集成之前，你需要：

#### 对于 Google Cloud 的 Agent Platform：

1. 一个已启用 Google Cloud Agent Platform 的 Google Cloud 项目
2. 已为 GitHub Actions 配置 Workload Identity Federation
3. 一个具备所需权限的服务账号
4. 一个 GitHub App（推荐）或使用默认的 `GITHUB_TOKEN`

#### 对于 Amazon Bedrock：

1. 一个已启用 Amazon Bedrock 的 AWS 账号
2. 在 AWS 中配置好 GitHub OIDC Identity Provider
3. 一个具备 Amazon Bedrock 权限的 IAM 角色
4. 一个 GitHub App（推荐）或使用默认的 `GITHUB_TOKEN`

### Create a custom GitHub App (Recommended for 3P Providers)

For best control and security when using 3P providers like Google Cloud's Agent Platform or Amazon Bedrock, we recommend creating your own GitHub App:

1. Go to [https://github.com/settings/apps/new](https://github.com/settings/apps/new)
2. Fill in the basic information:
   * **GitHub App name**: Choose a unique name (e.g., "YourOrg Claude Assistant")
   * **Homepage URL**: Your organization's website or the repository URL
3. Configure the app settings:
   * **Webhooks**: Uncheck "Active" (not needed for this integration)
4. Set the required permissions:
   * **Repository permissions**:
     * Contents: Read & Write
     * Issues: Read & Write
     * Pull requests: Read & Write
5. Click "Create GitHub App"
6. After creation, click "Generate a private key" and save the downloaded `.pem` file
7. Note your App ID from the app settings page
8. Install the app to your repository:
   * From your app's settings page, click "Install App" in the left sidebar
   * Select your account or organization
   * Choose "Only select repositories" and select the specific repository
   * Click "Install"
9. Add the private key as a secret to your repository:
   * Go to your repository's Settings → Secrets and variables → Actions
   * Create a new secret named `APP_PRIVATE_KEY` with the contents of the `.pem` file
10. Add the App ID as a secret:

* Create a new secret named `APP_ID` with your GitHub App's ID

<Note>
  This app will be used with the [actions/create-github-app-token](https://github.com/actions/create-github-app-token) action to generate authentication tokens in your workflows.
</Note>

**Alternative for Claude API or if you don't want to setup your own Github app**: Use the official Anthropic app:

1. Install from: [https://github.com/apps/claude](https://github.com/apps/claude)
2. No additional configuration needed for authentication

### 创建自定义 GitHub App（推荐第三方服务商使用）

当使用 Google Cloud 的 Agent Platform 或 Amazon Bedrock 等第三方服务商时，为获得最佳控制力与安全性，建议你创建自己的 GitHub App：

1. 访问 [https://github.com/settings/apps/new](https://github.com/settings/apps/new)
2. 填写基本信息：
   * **GitHub App name**：选择一个唯一的名称（例如 "YourOrg Claude Assistant"）
   * **Homepage URL**：你的组织网站或仓库 URL
3. 配置 App 设置：
   * **Webhooks**：取消勾选 "Active"（本集成不需要）
4. 设置所需权限：
   * **Repository permissions**：
     * Contents：Read & Write
     * Issues：Read & Write
     * Pull requests：Read & Write
5. 点击 "Create GitHub App"
6. 创建完成后，点击 "Generate a private key"，并保存下载的 `.pem` 文件
7. 在 App 设置页面记下你的 App ID
8. 将该 App 安装到你的仓库：
   * 在 App 设置页面左侧栏点击 "Install App"
   * 选择你的账号或组织
   * 选择 "Only select repositories" 并选中具体仓库
   * 点击 "Install"
9. 将私钥作为 secret 添加到你的仓库：
   * 进入仓库的 Settings → Secrets and variables → Actions
   * 新建名为 `APP_PRIVATE_KEY` 的 secret，内容为 `.pem` 文件内容
10. 将 App ID 也添加为 secret：

* 新建名为 `APP_ID` 的 secret，内容为你的 GitHub App 的 ID

<Note>
  该 App 将与 [actions/create-github-app-token](https://github.com/actions/create-github-app-token) 配合使用，在你的工作流中生成认证 token。
</Note>

**对于 Claude API 用户，或不想自建 GitHub App 的替代方案**：使用官方 Anthropic App：

1. 从此安装：[https://github.com/apps/claude](https://github.com/apps/claude)
2. 认证无需额外配置

### Configure cloud provider authentication

Choose your cloud provider and set up secure authentication:

### Amazon Bedrock

**Configure AWS to allow GitHub Actions to authenticate securely without storing credentials.**

> **Security Note**: Use repository-specific configurations and grant only the minimum required permissions.

**Required Setup**:

1. **Enable Amazon Bedrock**:
   * Request access to Claude models in Amazon Bedrock
   * For cross-region models, request access in all required regions

2. **Set up GitHub OIDC Identity Provider**:
   * Provider URL: `https://token.actions.githubusercontent.com`
   * Audience: `sts.amazonaws.com`

3. **Create IAM Role for GitHub Actions**:
   * Trusted entity type: Web identity
   * Identity provider: `token.actions.githubusercontent.com`
   * Permissions: `AmazonBedrockFullAccess` policy
   * Configure trust policy for your specific repository

**Required Values**:

After setup, you'll need:

* **AWS\_ROLE\_TO\_ASSUME**: The ARN of the IAM role you created

<Tip>
  OIDC is more secure than using static AWS access keys because credentials are temporary and automatically rotated.
</Tip>

See [AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html) for detailed OIDC setup instructions.

### 配置云服务商认证

选择你的云服务商并配置安全认证：

### Amazon Bedrock

**配置 AWS，使 GitHub Actions 能够在无需存储凭证的情况下安全认证。**

> **安全提示**：使用仓库专属配置，仅授予最低所需权限。

**所需配置**：

1. **启用 Amazon Bedrock**：
   * 在 Amazon Bedrock 中申请 Claude 模型的访问权限
   * 对于跨区域模型，需在所有所需区域申请访问权限

2. **设置 GitHub OIDC Identity Provider**：
   * Provider URL：`https://token.actions.githubusercontent.com`
   * Audience：`sts.amazonaws.com`

3. **为 GitHub Actions 创建 IAM 角色**：
   * 信任实体类型：Web identity
   * Identity provider：`token.actions.githubusercontent.com`
   * 权限：`AmazonBedrockFullAccess` 策略
   * 为你的具体仓库配置信任策略

**所需值**：

配置完成后，你需要：

* **`AWS_ROLE_TO_ASSUME`**：你所创建的 IAM 角色的 ARN

<Tip>
  OIDC 比使用静态 AWS access key 更安全，因为凭证是临时的且会自动轮换。
</Tip>

详细的 OIDC 配置说明请参阅 [AWS 文档](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_create_oidc.html)。

### Google Cloud's Agent Platform

**Configure Google Cloud to allow GitHub Actions to authenticate securely without storing credentials.**

> **Security Note**: Use repository-specific configurations and grant only the minimum required permissions.

**Required Setup**:

1. **Enable APIs** in your Google Cloud project:
   * IAM Credentials API
   * Security Token Service (STS) API
   * Google Cloud's Agent Platform API

2. **Create Workload Identity Federation resources**:
   * Create a Workload Identity Pool
   * Add a GitHub OIDC provider with:
     * Issuer: `https://token.actions.githubusercontent.com`
     * Attribute mappings for repository and owner
     * **Security recommendation**: Use repository-specific attribute conditions

3. **Create a Service Account**:
   * Grant only `Vertex AI User` role
   * **Security recommendation**: Create a dedicated service account per repository

4. **Configure IAM bindings**:
   * Allow the Workload Identity Pool to impersonate the service account
   * **Security recommendation**: Use repository-specific principal sets

**Required Values**:

After setup, you'll need:

* **GCP\_WORKLOAD\_IDENTITY\_PROVIDER**: The full provider resource name
* **GCP\_SERVICE\_ACCOUNT**: The service account email address

<Tip>
  Workload Identity Federation eliminates the need for downloadable service account keys, improving security.
</Tip>

For detailed setup instructions, consult the [Google Cloud Workload Identity Federation documentation](https://cloud.google.com/iam/docs/workload-identity-federation).

### Google Cloud 的 Agent Platform

**配置 Google Cloud，使 GitHub Actions 能够在无需存储凭证的情况下安全认证。**

> **安全提示**：使用仓库专属配置，仅授予最低所需权限。

**所需配置**：

1. **在你的 Google Cloud 项目中启用 API**：
   * IAM Credentials API
   * Security Token Service (STS) API
   * Google Cloud 的 Agent Platform API

2. **创建 Workload Identity Federation 资源**：
   * 创建一个 Workload Identity Pool
   * 添加一个 GitHub OIDC provider，配置如下：
     * Issuer：`https://token.actions.githubusercontent.com`
     * 针对仓库和 owner 的属性映射
     * **安全建议**：使用仓库专属的属性条件

3. **创建服务账号**：
   * 仅授予 `Vertex AI User` 角色
   * **安全建议**：为每个仓库创建独立的服务账号

4. **配置 IAM 绑定**：
   * 允许 Workload Identity Pool 模拟该服务账号
   * **安全建议**：使用仓库专属的 principal set

**所需值**：

配置完成后，你需要：

* **`GCP_WORKLOAD_IDENTITY_PROVIDER`**：完整的 provider 资源名
* **`GCP_SERVICE_ACCOUNT`**：服务账号邮箱地址

<Tip>
  Workload Identity Federation 无需下载服务账号密钥，从而提升安全性。
</Tip>

详细配置说明请参阅 [Google Cloud Workload Identity Federation 文档](https://cloud.google.com/iam/docs/workload-identity-federation)。

### Add Required Secrets

Add the following secrets to your repository (Settings → Secrets and variables → Actions):

#### For Claude API (Direct):

1. **For API Authentication**:
   * `ANTHROPIC_API_KEY`: Your Claude API key from [console.anthropic.com](https://console.anthropic.com)

2. **For GitHub App (if using your own app)**:
   * `APP_ID`: Your GitHub App's ID
   * `APP_PRIVATE_KEY`: The private key (.pem) content

#### For Google Cloud's Agent Platform

1. **For GCP Authentication**:
   * `GCP_WORKLOAD_IDENTITY_PROVIDER`
   * `GCP_SERVICE_ACCOUNT`

2. **For GitHub App (if using your own app)**:
   * `APP_ID`: Your GitHub App's ID
   * `APP_PRIVATE_KEY`: The private key (.pem) content

#### For Amazon Bedrock

1. **For AWS Authentication**:
   * `AWS_ROLE_TO_ASSUME`

2. **For GitHub App (if using your own app)**:
   * `APP_ID`: Your GitHub App's ID
   * `APP_PRIVATE_KEY`: The private key (.pem) content

### 添加所需 Secrets

将以下 secrets 添加到你的仓库（Settings → Secrets and variables → Actions）：

#### 对于 Claude API（直连）：

1. **用于 API 认证**：
   * `ANTHROPIC_API_KEY`：来自 [console.anthropic.com](https://console.anthropic.com) 的 Claude API key

2. **用于 GitHub App（若使用自建 App）**：
   * `APP_ID`：你的 GitHub App 的 ID
   * `APP_PRIVATE_KEY`：私钥（.pem）内容

#### 对于 Google Cloud 的 Agent Platform

1. **用于 GCP 认证**：
   * `GCP_WORKLOAD_IDENTITY_PROVIDER`
   * `GCP_SERVICE_ACCOUNT`

2. **用于 GitHub App（若使用自建 App）**：
   * `APP_ID`：你的 GitHub App 的 ID
   * `APP_PRIVATE_KEY`：私钥（.pem）内容

#### 对于 Amazon Bedrock

1. **用于 AWS 认证**：
   * `AWS_ROLE_TO_ASSUME`

2. **用于 GitHub App（若使用自建 App）**：
   * `APP_ID`：你的 GitHub App 的 ID
   * `APP_PRIVATE_KEY`：私钥（.pem）内容

### Create workflow files

Create GitHub Actions workflow files that integrate with your cloud provider. The examples below show complete configurations for both Amazon Bedrock and Google Cloud's Agent Platform:

### Amazon Bedrock workflow

**Prerequisites:**

* Amazon Bedrock access enabled with Claude model permissions
* GitHub configured as an OIDC identity provider in AWS
* IAM role with Amazon Bedrock permissions that trusts GitHub Actions

**Required GitHub secrets:**

| Secret Name          | Description                                       |
| -------------------- | ------------------------------------------------- |
| `AWS_ROLE_TO_ASSUME` | ARN of the IAM role for Amazon Bedrock access     |
| `APP_ID`             | Your GitHub App ID (from app settings)            |
| `APP_PRIVATE_KEY`    | The private key you generated for your GitHub App |

### 创建工作流文件

创建与你的云服务商集成的 GitHub Actions 工作流文件。下方示例展示了 Amazon Bedrock 和 Google Cloud Agent Platform 的完整配置：

### Amazon Bedrock 工作流

**前置条件：**

* 已启用 Amazon Bedrock 访问并具备 Claude 模型权限
* 在 AWS 中将 GitHub 配置为 OIDC identity provider
* 拥有信任 GitHub Actions 且具备 Amazon Bedrock 权限的 IAM 角色

**所需 GitHub secrets：**

| Secret 名称           | 描述                                              |
| -------------------- | ------------------------------------------------- |
| `AWS_ROLE_TO_ASSUME` | 用于 Amazon Bedrock 访问的 IAM 角色 ARN           |
| `APP_ID`             | 你的 GitHub App ID（来自 App 设置页面）           |
| `APP_PRIVATE_KEY`    | 你为 GitHub App 生成的私钥                        |

```yaml theme={null}
name: Claude PR Action

permissions:
  contents: write
  pull-requests: write
  issues: write
  id-token: write

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]

jobs:
  claude-pr:
    if: |
      (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'issues' && contains(github.event.issue.body, '@claude'))
    runs-on: ubuntu-latest
    env:
      AWS_REGION: us-west-2
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Generate GitHub App token
        id: app-token
        uses: actions/create-github-app-token@v2
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}

      - name: Configure AWS Credentials (OIDC)
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_TO_ASSUME }}
          aws-region: us-west-2

      - uses: anthropics/claude-code-action@v1
        with:
          github_token: ${{ steps.app-token.outputs.token }}
          use_bedrock: "true"
          claude_args: '--model us.anthropic.claude-sonnet-4-6 --max-turns 10'
```

<Tip>
  The model ID format for Amazon Bedrock includes a region prefix (for example, `us.anthropic.claude-sonnet-4-6`).
</Tip>

<Tip>
  Amazon Bedrock 的 model ID 格式带有区域前缀（例如 `us.anthropic.claude-sonnet-4-6`）。
</Tip>

### Google Cloud's Agent Platform workflow

**Prerequisites:**

* Google Cloud's Agent Platform API enabled in your GCP project
* Workload Identity Federation configured for GitHub
* Service account with Google Cloud's Agent Platform permissions

**Required GitHub secrets:**

| Secret Name                      | Description                                                     |
| -------------------------------- | --------------------------------------------------------------- |
| `GCP_WORKLOAD_IDENTITY_PROVIDER` | Workload identity provider resource name                        |
| `GCP_SERVICE_ACCOUNT`            | Service account email with Google Cloud's Agent Platform access |
| `APP_ID`                         | Your GitHub App ID (from app settings)                          |
| `APP_PRIVATE_KEY`                | The private key you generated for your GitHub App               |

### Google Cloud 的 Agent Platform 工作流

**前置条件：**

* 在你的 GCP 项目中已启用 Google Cloud 的 Agent Platform API
* 已为 GitHub 配置 Workload Identity Federation
* 拥有具备 Google Cloud Agent Platform 权限的服务账号

**所需 GitHub secrets：**

| Secret 名称                      | 描述                                                            |
| -------------------------------- | --------------------------------------------------------------- |
| `GCP_WORKLOAD_IDENTITY_PROVIDER` | Workload identity provider 资源名                               |
| `GCP_SERVICE_ACCOUNT`            | 具备 Google Cloud Agent Platform 访问权限的服务账号邮箱         |
| `APP_ID`                         | 你的 GitHub App ID（来自 App 设置页面）                         |
| `APP_PRIVATE_KEY`                | 你为 GitHub App 生成的私钥                                      |

```yaml theme={null}
name: Claude PR Action

permissions:
  contents: write
  pull-requests: write
  issues: write
  id-token: write

on:
  issue_comment:
    types: [created]
  pull_request_review_comment:
    types: [created]
  issues:
    types: [opened, assigned]

jobs:
  claude-pr:
    if: |
      (github.event_name == 'issue_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'pull_request_review_comment' && contains(github.event.comment.body, '@claude')) ||
      (github.event_name == 'issues' && contains(github.event.issue.body, '@claude'))
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      - name: Generate GitHub App token
        id: app-token
        uses: actions/create-github-app-token@v2
        with:
          app-id: ${{ secrets.APP_ID }}
          private-key: ${{ secrets.APP_PRIVATE_KEY }}

      - name: Authenticate to Google Cloud
        id: auth
        uses: google-github-actions/auth@v2
        with:
          workload_identity_provider: ${{ secrets.GCP_WORKLOAD_IDENTITY_PROVIDER }}
          service_account: ${{ secrets.GCP_SERVICE_ACCOUNT }}

      - uses: anthropics/claude-code-action@v1
        with:
          github_token: ${{ steps.app-token.outputs.token }}
          trigger_phrase: "@claude"
          use_vertex: "true"
          claude_args: '--model claude-sonnet-4-5@20250929 --max-turns 10'
        env:
          ANTHROPIC_VERTEX_PROJECT_ID: ${{ steps.auth.outputs.project_id }}
          CLOUD_ML_REGION: us-east5
          VERTEX_REGION_CLAUDE_4_5_SONNET: us-east5
```

<Tip>
  The project ID is automatically retrieved from the Google Cloud authentication step, so you don't need to hardcode it.
</Tip>

<Tip>
  project ID 会从 Google Cloud 认证步骤中自动获取，因此无需硬编码。
</Tip>

### Troubleshooting

### Claude not responding to @claude commands

Verify the GitHub App is installed correctly, check that workflows are enabled, ensure API key is set in repository secrets, and confirm the comment contains `@claude` (not `/claude`).

### 故障排查

### Claude 不响应 @claude 命令

请确认 GitHub App 是否正确安装、工作流是否已启用、API key 是否已设置在仓库 secrets 中，并确认评论中包含的是 `@claude`（而非 `/claude`）。

### CI not running on Claude's commits

Ensure you're using the GitHub App or custom app (not Actions user), check workflow triggers include the necessary events, and verify app permissions include CI triggers.

### Claude 提交后 CI 未运行

请确保使用的是 GitHub App 或自建 App（而非 Actions 用户），检查工作流触发器是否包含所需事件，并确认 App 权限中包含 CI 触发权限。

### Authentication errors

Confirm API key is valid and has sufficient permissions. For Amazon Bedrock or Google Cloud's Agent Platform, check credentials configuration and ensure secrets are named correctly in workflows.

### 认证错误

请确认 API key 有效且具备足够权限。对于 Amazon Bedrock 或 Google Cloud 的 Agent Platform，请检查凭证配置，并确保工作流中的 secret 名称正确。

### Advanced configuration

### Action parameters

The Claude Code Action v1 uses a simplified configuration:

| Parameter             | Description                                                        | Required |
| --------------------- | ------------------------------------------------------------------ | -------- |
| `prompt`              | Instructions for Claude (plain text or a [skill](/en/skills) name) | No\*     |
| `claude_args`         | CLI arguments passed to Claude Code                                | No       |
| `plugin_marketplaces` | Newline-separated list of plugin marketplace Git URLs              | No       |
| `plugins`             | Newline-separated list of plugin names to install before execution | No       |
| `anthropic_api_key`   | Claude API key                                                     | Yes\*\*  |
| `github_token`        | GitHub token for API access                                        | No       |
| `trigger_phrase`      | Custom trigger phrase (default: "@claude")                         | No       |
| `use_bedrock`         | Use Amazon Bedrock instead of Claude API                           | No       |
| `use_vertex`          | Use Google Cloud's Agent Platform instead of Claude API            | No       |

\*Prompt is optional - when omitted for issue/PR comments, Claude responds to trigger phrase\
\*\*Required for direct Claude API, not for Amazon Bedrock or Google Cloud's Agent Platform

### 高级配置

### Action 参数

Claude Code Action v1 采用简化配置：

| 参数                  | 描述                                                               | 是否必填 |
| --------------------- | ------------------------------------------------------------------ | -------- |
| `prompt`              | 给 Claude 的指令（纯文本或 [skill](/en/skills) 名称）             | 否\*     |
| `claude_args`         | 传给 Claude Code 的 CLI 参数                                       | 否       |
| `plugin_marketplaces` | 以换行分隔的 plugin marketplace Git URL 列表                       | 否       |
| `plugins`             | 以换行分隔的、执行前要安装的 plugin 名称列表                       | 否       |
| `anthropic_api_key`   | Claude API key                                                     | 是\*\*   |
| `github_token`        | 用于 API 访问的 GitHub token                                       | 否       |
| `trigger_phrase`      | 自定义触发短语（默认为 "@claude"）                                 | 否       |
| `use_bedrock`         | 使用 Amazon Bedrock 替代 Claude API                                | 否       |
| `use_vertex`          | 使用 Google Cloud 的 Agent Platform 替代 Claude API                | 否       |

\*prompt 为可选 —— 在 issue/PR 评论场景下省略时，Claude 会响应触发短语\
\*\*直连 Claude API 时必填，使用 Amazon Bedrock 或 Google Cloud 的 Agent Platform 时不需要

#### Pass CLI arguments

The `claude_args` parameter accepts any Claude Code CLI arguments:

```yaml theme={null}
claude_args: "--max-turns 5 --model claude-sonnet-5 --mcp-config /path/to/config.json"
```

Common arguments:

* `--max-turns`: Maximum conversation turns (default: 10)
* `--model`: Model to use (for example, `claude-sonnet-5`)
* `--mcp-config`: Path to MCP configuration
* `--allowedTools`: Comma-separated list of allowed tools. The `--allowed-tools` alias also works.
* `--debug`: Enable debug output

#### 传入 CLI 参数

`claude_args` 参数接受任意 Claude Code CLI 参数：

```yaml theme={null}
claude_args: "--max-turns 5 --model claude-sonnet-5 --mcp-config /path/to/config.json"
```

常用参数：

* `--max-turns`：最大对话轮数（默认：10）
* `--model`：使用的模型（例如 `claude-sonnet-5`）
* `--mcp-config`：MCP 配置文件路径
* `--allowedTools`：以逗号分隔的允许工具列表，也支持 `--allowed-tools` 别名
* `--debug`：启用调试输出

### Alternative integration methods

While the `/install-github-app` command is the recommended approach, you can also:

* **Custom GitHub App**: For organizations needing branded usernames or custom authentication flows. Create your own GitHub App with required permissions (contents, issues, pull requests) and use the actions/create-github-app-token action to generate tokens in your workflows.
* **Manual GitHub Actions**: Direct workflow configuration for maximum flexibility
* **MCP Configuration**: Dynamic loading of Model Context Protocol servers

See the [Claude Code Action documentation](https://github.com/anthropics/claude-code-action/blob/main/docs) for detailed guides on authentication, security, and advanced configuration.

### 其他集成方式

虽然 `/install-github-app` 命令是推荐方式，你也可以选择：

* **自定义 GitHub App**：适用于需要品牌化用户名或自定义认证流程的组织。创建一个具备所需权限（contents、issues、pull requests）的 GitHub App，并在工作流中使用 actions/create-github-app-token 生成 token。
* **手动 GitHub Actions**：直接配置工作流，获得最大灵活性
* **MCP 配置**：动态加载 Model Context Protocol 服务器

关于认证、安全和高级配置的详细指南，请参阅 [Claude Code Action 文档](https://github.com/anthropics/claude-code-action/blob/main/docs)。

### Customizing Claude's behavior

You can configure Claude's behavior in two ways:

1. **CLAUDE.md**: Define coding standards, review criteria, and project-specific rules in a `CLAUDE.md` file at the root of your repository. Claude will follow these guidelines when creating PRs and responding to requests. Check out our [Memory documentation](/en/memory) for more details.
2. **Custom prompts**: Use the `prompt` parameter in the workflow file to provide workflow-specific instructions. This allows you to customize Claude's behavior for different workflows or tasks.

Claude will follow these guidelines when creating PRs and responding to requests.

### 自定义 Claude 的行为

你可以通过两种方式配置 Claude 的行为：

1. **CLAUDE.md**：在仓库根目录的 `CLAUDE.md` 文件中定义编码规范、评审标准和项目专属规则。Claude 在创建 PR 和响应请求时会遵循这些指南。更多细节请参阅我们的 [Memory 文档](/en/memory)。
2. **自定义 prompt**：在工作流文件中使用 `prompt` 参数提供工作流专属指令。这样你可以针对不同工作流或任务自定义 Claude 的行为。

Claude 在创建 PR 和响应请求时会遵循这些指南。


---

## OpenCode - Introduction（简介）

原文链接：https://opencode.ai/docs


### Intro

Get started with OpenCode.

[**OpenCode**](/) is an open source AI coding agent. It's available as a terminal-based interface, desktop app, or IDE extension.

开始使用 OpenCode。

[**OpenCode**](/) 是一款开源的 AI coding agent（AI 编码代理）。它以终端界面、桌面应用或 IDE 扩展的形式提供。

（此处为 OpenCode TUI 终端界面截图，展示了 opencode 主题的界面外观。）

Let's get started.

让我们开始吧。

### Prerequisites

To use OpenCode in your terminal, you'll need:

1. A modern terminal emulator like:
    - [WezTerm](https://wezterm.org), cross-platform
    - [Alacritty](https://alacritty.org), cross-platform
    - [Ghostty](https://ghostty.org), Linux and macOS
    - [Kitty](https://sw.kovidgoyal.net/kitty/), Linux and macOS
2. API keys for the LLM providers you want to use.

要在终端中使用 OpenCode，你需要：

1. 一款现代的终端模拟器，例如：
    - [WezTerm](https://wezterm.org)，跨平台
    - [Alacritty](https://alacritty.org)，跨平台
    - [Ghostty](https://ghostty.org)，Linux 和 macOS
    - [Kitty](https://sw.kovidgoyal.net/kitty/)，Linux 和 macOS
2. 你想使用的 LLM provider（大语言模型供应商）的 API key（API 密钥）。

### Install

The easiest way to install OpenCode is through the install script.

安装 OpenCode 最简单的方式是通过安装脚本。

```
curl -fsSL https://opencode.ai/install | bash
```

You can also install it with the following commands:

你也可以使用以下命令来安装：

- **Using Node.js**

    ```
    npm install -g opencode-ai
    ```

- **Using Node.js**

    ```
    bun install -g opencode-ai
    ```

- **Using Node.js**

    ```
    pnpm install -g opencode-ai
    ```

- **Using Node.js**

    ```
    yarn global add opencode-ai
    ```

- **Using Node.js**（使用 Node.js 安装）

    ```
    npm install -g opencode-ai
    ```

    ```
    bun install -g opencode-ai
    ```

    ```
    pnpm install -g opencode-ai
    ```

    ```
    yarn global add opencode-ai
    ```

- **Using Homebrew on macOS and Linux**

    ```
    brew install anomalyco/tap/opencode
    ```

- **在 macOS 和 Linux 上使用 Homebrew**

    ```
    brew install anomalyco/tap/opencode
    ```

> We recommend using the OpenCode tap for the most up to date releases. The official `brew install opencode` formula is maintained by the Homebrew team and is updated less frequently.

> 我们推荐使用 OpenCode 的 tap 来获取最新的发布版本。官方的 `brew install opencode` formula 由 Homebrew 团队维护，更新频率较低。

- **Installing on Arch Linux**

    ```
    sudo pacman -S opencode           # Arch Linux (Stable)
    paru -S opencode-bin              # Arch Linux (Latest from AUR)
    ```

- **在 Arch Linux 上安装**

    ```
    sudo pacman -S opencode           # Arch Linux (Stable)
    paru -S opencode-bin              # Arch Linux (Latest from AUR)
    ```

#### Windows

Recommended: Use WSL

For the best experience on Windows, we recommend using [Windows Subsystem for Linux (WSL)](/docs/windows-wsl). It provides better performance and full compatibility with OpenCode's features.

#### Windows

推荐：使用 WSL

为了在 Windows 上获得最佳体验，我们推荐使用 [Windows Subsystem for Linux (WSL)](/docs/windows-wsl)（Windows 的 Linux 子系统）。它能提供更好的性能，并与 OpenCode 的各项功能完全兼容。

- **Using Chocolatey**

    ```
    choco install opencode
    ```

- **Using Scoop**

    ```
    scoop install opencode
    ```

- **Using NPM**

    ```
    npm install -g opencode-ai
    ```

- **Using Mise**

    ```
    mise use -g github:anomalyco/opencode
    ```

- **Using Docker**

    ```
    docker run -it --rm ghcr.io/anomalyco/opencode
    ```

- **使用 Chocolatey**

    ```
    choco install opencode
    ```

- **使用 Scoop**

    ```
    scoop install opencode
    ```

- **使用 NPM**

    ```
    npm install -g opencode-ai
    ```

- **使用 Mise**

    ```
    mise use -g github:anomalyco/opencode
    ```

- **使用 Docker**

    ```
    docker run -it --rm ghcr.io/anomalyco/opencode
    ```

Support for installing OpenCode on Windows using Bun is currently in progress.

关于在 Windows 上使用 Bun 安装 OpenCode 的支持目前正在开发中。

You can also grab the binary from the [Releases](https://github.com/anomalyco/opencode/releases).

你也可以直接从 [Releases](https://github.com/anomalyco/opencode/releases) 页面获取二进制文件。

### Configure

With OpenCode you can use any LLM provider by configuring their API keys.

在 OpenCode 中，你可以通过配置 API key 来使用任意 LLM provider。

If you are new to using LLM providers, we recommend using [OpenCode Zen](/docs/zen). It's a curated list of models that have been tested and verified by the OpenCode team.

如果你刚开始接触 LLM provider，我们推荐使用 [OpenCode Zen](/docs/zen)。这是一份经过 OpenCode 团队测试和验证的精选模型列表。

1. Run the `/connect` command in the TUI, select opencode, and head to [opencode.ai/auth](https://opencode.ai/auth).

    ```
    /connect
    ```

2. Sign in, add your billing details, and copy your API key.

3. Paste your API key.

    ```
    ┌ API key
    │
    │
    └ enter
    ```

1. 在 TUI 中运行 `/connect` 命令，选择 opencode，然后前往 [opencode.ai/auth](https://opencode.ai/auth)。

    ```
    /connect
    ```

2. 登录、填写账单信息，并复制你的 API key。

3. 粘贴你的 API key。

    ```
    ┌ API key
    │
    │
    └ enter
    ```

Alternatively, you can select one of the other providers. [Learn more](/docs/providers#directory).

或者，你也可以选择其他 provider 之一。[了解更多](/docs/providers#directory)。

### Initialize

Now that you've configured a provider, you can navigate to a project that you want to work on.

现在你已经配置好了 provider，可以进入你想要处理的项目目录。

```
cd /path/to/project
```

And run OpenCode.

然后运行 OpenCode。

```
opencode
```

Next, initialize OpenCode for the project by running the following command.

接下来，运行以下命令为该项目初始化 OpenCode。

```
/init
```

This will get OpenCode to analyze your project and create an `AGENTS.md` file in the project root.

这会让 OpenCode 分析你的项目，并在项目根目录下创建一个 `AGENTS.md` 文件。

Tip

You should commit your project's `AGENTS.md` file to Git.

This helps OpenCode understand the project structure and the coding patterns used.

提示

你应该将项目的 `AGENTS.md` 文件提交到 Git。

这有助于 OpenCode 理解项目结构以及所使用的编码模式。

### Usage

You are now ready to use OpenCode to work on your project. Feel free to ask it anything!

If you are new to using an AI coding agent, here are some examples that might help.

现在你已经准备好使用 OpenCode 来处理你的项目了。尽管问它任何问题！

如果你刚开始使用 AI coding agent，下面这些示例或许能帮到你。

### Ask questions

You can ask OpenCode to explain the codebase to you.

Tip

Use the `@` key to fuzzy search for files in the project.

### 提问

你可以让 OpenCode 为你解释代码库。

提示

使用 `@` 键可以在项目中模糊搜索文件。

```
How is authentication handled in @packages/functions/src/api/index.ts
```

This is helpful if there's a part of the codebase that you didn't work on.

当代码库中存在你未曾参与的部分时，这会很有帮助。

### Add features

You can ask OpenCode to add new features to your project. Though we first recommend asking it to create a plan.

### 添加功能

你可以让 OpenCode 为你的项目添加新功能。不过我们首先建议让它先制定一个计划。

1. **Create a plan**

    OpenCode has a *Plan mode* that disables its ability to make changes and instead suggest *how* it'll implement the feature.

    Switch to it using the **Tab** key. You'll see an indicator for this in the lower right corner.

1. **创建计划**

    OpenCode 有一个 *Plan mode*（计划模式），在该模式下它会关闭直接修改代码的能力，转而建议它将*如何*实现该功能。

    使用 **Tab** 键切换到该模式。你会在右下角看到一个对应的指示器。

    ```
    <TAB>
    ```

    Now let's describe what we want it to do.

    现在让我们描述一下希望它做的事情。

    ```
    When a user deletes a note, we'd like to flag it as deleted in the database.
    Then create a screen that shows all the recently deleted notes.
    From this screen, the user can undelete a note or permanently delete it.
    ```

    You want to give OpenCode enough details to understand what you want. It helps to talk to it like you are talking to a junior developer on your team.

    Tip

    Give OpenCode plenty of context and examples to help it understand what you want.

    你需要给 OpenCode 提供足够的细节，让它理解你的需求。把它当作你团队中的一名初级开发者来沟通，效果会很好。

    提示

    给 OpenCode 提供充足的上下文和示例，有助于它理解你的需求。

2. **Iterate on the plan**

    Once it gives you a plan, you can give it feedback or add more details.

2. **迭代计划**

    当它给出计划后，你可以给它反馈或补充更多细节。

    ```
    We'd like to design this new screen using a design I've used before.
    [Image #1] Take a look at this image and use it as a reference.
    ```

    Tip

    Drag and drop images into the terminal to add them to the prompt.

    提示

    将图片拖放到终端中，即可将其添加到 prompt（提示词）中。

    OpenCode can scan any images you give it and add them to the prompt. You can do this by dragging and dropping an image into the terminal.

    OpenCode 可以扫描你提供的任何图片，并将它们加入 prompt。你可以通过将图片拖放到终端中来实现。

3. **Build the feature**

    Once you feel comfortable with the plan, switch back to *Build mode* by hitting the **Tab** key again.

3. **构建功能**

    当你对计划感到满意后，再次按下 **Tab** 键切换回 *Build mode*（构建模式）。

    ```
    <TAB>
    ```

    And asking it to make the changes.

    然后让它执行修改。

    ```
    Sounds good! Go ahead and make the changes.
    ```

### Make changes

For more straightforward changes, you can ask OpenCode to directly build it without having to review the plan first.

### 修改代码

对于较为简单的修改，你可以直接让 OpenCode 构建，而无需先审阅计划。

```
We need to add authentication to the /settings route. Take a look at how this is
handled in the /notes route in @packages/functions/src/notes.ts and implement
the same logic in @packages/functions/src/settings.ts
```

You want to make sure you provide a good amount of detail so OpenCode makes the right changes.

你需要确保提供足够详细的描述，这样 OpenCode 才能做出正确的修改。

### Undo changes

Let's say you ask OpenCode to make some changes.

### 撤销修改

假设你让 OpenCode 做了一些修改。

```
Can you refactor the function in @packages/functions/src/api/index.ts?
```

But you realize that it is not what you wanted. You **can undo** the changes using the `/undo` command.

但你发现这并不是你想要的结果。你**可以撤销**这些修改，方法是使用 `/undo` 命令。

```
/undo
```

OpenCode will now revert the changes you made and show your original message again.

OpenCode 此时会撤销你所做的修改，并重新显示你最初发送的消息。

```
Can you refactor the function in @packages/functions/src/api/index.ts?
```

From here you can tweak the prompt and ask OpenCode to try again.

你可以在此处调整 prompt，然后让 OpenCode 重新尝试。

Tip

You can run `/undo` multiple times to undo multiple changes.

提示

你可以多次运行 `/undo` 来撤销多次修改。

Or you **can redo** the changes using the `/redo` command.

或者你**可以重做**这些修改，方法是使用 `/redo` 命令。

```
/redo
```

### Share

The conversations that you have with OpenCode can be [shared with your team](/docs/share).

### 分享

你与 OpenCode 的对话可以[与你的团队分享](/docs/share)。

```
/share
```

This will create a link to the current conversation and copy it to your clipboard.

这会为当前对话生成一个链接，并将其复制到你的剪贴板。

Note

Conversations are not shared by default.

注意

对话默认不会被分享。

Here's an [example conversation](https://opencode.ai/s/4XP1fce5) with OpenCode.

这里有一个与 OpenCode 的[示例对话](https://opencode.ai/s/4XP1fce5)。

### Customize

And that's it! You are now a pro at using OpenCode.

### 个性化定制

就这些！你现在已经是使用 OpenCode 的高手了。

To make it your own, we recommend [picking a theme](/docs/themes), [customizing the keybinds](/docs/keybinds), [configuring code formatters](/docs/formatters), [creating custom commands](/docs/commands), or playing around with the [OpenCode config](/docs/config).

为了让它真正属于你，我们推荐你[选择一个主题](/docs/themes)、[自定义快捷键](/docs/keybinds)、[配置代码格式化工具](/docs/formatters)、[创建自定义命令](/docs/commands)，或者尝试一下 [OpenCode 配置](/docs/config)。


---

## OpenCode - Agents（智能体）

原文链接：https://opencode.ai/docs/agents


### Overview

Configure and use specialized agents.

Agents are specialized AI assistants that can be configured for specific tasks and workflows. They allow you to create focused tools with custom prompts, models, and tool access.

Tip

Use the plan agent to analyze code and review suggestions without making any code changes.

You can switch between agents during a session or invoke them with the `@` mention.

配置并使用专门的代理（Agent，可针对特定任务配置的 AI 助手）。

代理是可针对特定任务和工作流（Workflow，任务流程）进行配置的专用 AI 助手。它们允许你创建带有自定义提示词（Prompt，给模型的指令文本）、模型和工具访问权限的聚焦工具。

提示

使用 plan 代理来分析代码并审查建议，而无需做任何代码改动。

你可以在会话期间切换代理，或通过 `@` 提及来调用它们。

---

### Types

There are two types of agents in OpenCode; primary agents and subagents.

### 类型

OpenCode 中有两种类型的代理：主代理（Primary Agent，你直接交互的主要助手）和子代理（Subagent，由主代理调用的专用助手）。

---

### Primary agents

Primary agents are the main assistants you interact with directly. You can cycle through them using the **Tab** key, or your configured `switch_agent` keybind. These agents handle your main conversation. Tool access is configured via permissions — for example, Build has all tools enabled while Plan is restricted.

Tip

You can use the **Tab** key to switch between primary agents during a session.

OpenCode comes with two built-in primary agents, **Build** and **Plan**. We'll look at these below.

### 主代理

主代理是你直接交互的主要助手。你可以使用 **Tab** 键，或你配置的 `switch_agent` 快捷键在它们之间循环切换。这些代理处理你的主对话。工具访问通过权限配置——例如，Build 启用了所有工具，而 Plan 则受到限制。

提示

你可以在会话期间使用 **Tab** 键在主代理之间切换。

OpenCode 自带两个内置主代理，**Build** 和 **Plan**。我们将在下面介绍它们。

---

### Subagents

Subagents are specialized assistants that primary agents can invoke for specific tasks. You can also manually invoke them by **@ mentioning** them in your messages.

OpenCode comes with three built-in subagents, **General**, **Explore**, and **Scout**. We'll look at this below.

### 子代理

子代理是主代理可针对特定任务调用的专用助手。你也可以在消息中通过 **@ 提及** 手动调用它们。

OpenCode 自带三个内置子代理，**General**、**Explore** 和 **Scout**。我们将在下面介绍它们。

---

### Built-in

OpenCode comes with two built-in primary agents and three built-in subagents.

### 内置

OpenCode 自带两个内置主代理和三个内置子代理。

---

### Use build

*Mode*: `primary`

Build is the **default** primary agent with all tools enabled. This is the standard agent for development work where you need full access to file operations and system commands.

### 使用 build

*模式*：`primary`

Build 是**默认**主代理，启用了所有工具。这是用于开发工作的标准代理，适用于你需要完全访问文件操作和系统命令的场景。

---

### Use plan

*Mode*: `primary`

A restricted agent designed for planning and analysis. We use a permission system to give you more control and prevent unintended changes. By default, all of the following are set to `ask`:

-   `file edits`: All writes, patches, and edits
-   `bash`: All bash commands

This agent is useful when you want the LLM to analyze code, suggest changes, or create plans without making any actual modifications to your codebase.

### 使用 plan

*模式*：`primary`

一个为规划和分析而设计的受限代理。我们使用权限系统来给你更多控制权，并防止意外改动。默认情况下，以下所有项都设置为 `ask`：

-   `file edits`：所有写入、补丁和编辑
-   `bash`：所有 bash 命令

当你希望 LLM 分析代码、提出改动建议或创建计划，而不对代码库做任何实际修改时，这个代理很有用。

---

### Use general

*Mode*: `subagent`

A general-purpose agent for researching complex questions and executing multi-step tasks. Has full tool access (except todo), so it can make file changes when needed. Use this to run multiple units of work in parallel.

### 使用 general

*模式*：`subagent`

一个通用代理，用于研究复杂问题并执行多步任务。拥有完整的工具访问权限（todo 除外），因此可以在需要时修改文件。用它来并行运行多个工作单元。

---

### Use explore

*Mode*: `subagent`

A fast, read-only agent for exploring codebases. Cannot modify files. Use this when you need to quickly find files by patterns, search code for keywords, or answer questions about the codebase.

### 使用 explore

*模式*：`subagent`

一个快速、只读的代理，用于探索代码库。不能修改文件。当你需要按模式快速查找文件、在代码中搜索关键字或回答关于代码库的问题时，使用它。

---

### Use scout

*Mode*: `subagent`

A read-only agent for external docs and dependency research. Use this when you need to clone a dependency repository into OpenCode's managed cache, inspect library source, or cross-reference local code against upstream implementations without modifying your workspace.

### 使用 scout

*模式*：`subagent`

一个只读代理，用于外部文档和依赖项研究。当你需要将依赖仓库克隆到 OpenCode 的托管缓存中、检查库源码，或在不动你工作区的前提下将本地代码与上游实现进行交叉比对时，使用它。

---

### Use compaction

*Mode*: `primary`

Hidden system agent that compacts long context into a smaller summary. It runs automatically when needed and is not selectable in the UI.

### 使用 compaction

*模式*：`primary`

隐藏的系统代理，将长上下文压缩为更小的摘要。它在需要时自动运行，且在 UI 中不可选择。

---

### Use title

*Mode*: `primary`

Hidden system agent that generates short session titles. It runs automatically and is not selectable in the UI.

### 使用 title

*模式*：`primary`

隐藏的系统代理，用于生成简短的会话标题。它自动运行，且在 UI 中不可选择。

---

### Use summary

*Mode*: `primary`

Hidden system agent that creates session summaries. It runs automatically and is not selectable in the UI.

### 使用 summary

*模式*：`primary`

隐藏的系统代理，用于创建会话摘要。它自动运行，且在 UI 中不可选择。

---

### Usage

1.  For primary agents, use the **Tab** key to cycle through them during a session. You can also use your configured `switch_agent` keybind.

2.  Subagents can be invoked:

    -   **Automatically** by primary agents for specialized tasks based on their descriptions.

    -   Manually by **@ mentioning** a subagent in your message. For example.

        ```
        @general help me search for this function
        ```

3.  **Navigation between sessions**: When subagents create child sessions, use `session_child_first` (default: **<Leader>+Down**) to enter the first child session from the parent.

4.  Once you are in a child session, use:

    -   `session_child_cycle` (default: **Right**) to cycle to the next child session
    -   `session_child_cycle_reverse` (default: **Left**) to cycle to the previous child session
    -   `session_parent` (default: **Up**) to return to the parent session

    This lets you switch between the main conversation and specialized subagent work.

### 用法

1.  对于主代理，在会话期间使用 **Tab** 键循环切换。你也可以使用配置的 `switch_agent` 快捷键。

2.  子代理可通过以下方式调用：

    -   **自动**：主代理根据其描述为特定任务调用子代理。

    -   手动：在消息中 **@ 提及** 某个子代理。例如：

        ```
        @general help me search for this function
        ```

3.  **会话之间导航**：当子代理创建子会话时，使用 `session_child_first`（默认：**<Leader>+Down**）从父会话进入第一个子会话。

4.  进入子会话后，使用：

    -   `session_child_cycle`（默认：**Right**）循环到下一个子会话
    -   `session_child_cycle_reverse`（默认：**Left**）循环到上一个子会话
    -   `session_parent`（默认：**Up**）返回父会话

    这样你就可以在主对话和专门的子代理工作之间切换。

---

### Configure

You can customize the built-in agents or create your own through configuration. Agents can be configured in two ways:

### 配置

你可以通过配置自定义内置代理或创建自己的代理。代理可以通过两种方式配置：

---

### JSON

Configure agents in your `opencode.json` config file:

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "mode": "primary",      "model": "anthropic/claude-sonnet-4-20250514",      "prompt": "{file:./prompts/build.txt}",      "permission": {        "edit": "allow",        "bash": "allow"      }    },    "plan": {      "mode": "primary",      "model": "anthropic/claude-haiku-4-20250514",      "permission": {        "edit": "deny",        "bash": "deny"      }    },    "code-reviewer": {      "description": "Reviews code for best practices and potential issues",      "mode": "subagent",      "model": "anthropic/claude-sonnet-4-20250514",      "prompt": "You are a code reviewer. Focus on security, performance, and maintainability.",      "permission": {        "edit": "deny"      }    }  }}
```

### JSON

在你的 `opencode.json` 配置文件中配置代理：

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "mode": "primary",      "model": "anthropic/claude-sonnet-4-20250514",      "prompt": "{file:./prompts/build.txt}",      "permission": {        "edit": "allow",        "bash": "allow"      }    },    "plan": {      "mode": "primary",      "model": "anthropic/claude-haiku-4-20250514",      "permission": {        "edit": "deny",        "bash": "deny"      }    },    "code-reviewer": {      "description": "Reviews code for best practices and potential issues",      "mode": "subagent",      "model": "anthropic/claude-sonnet-4-20250514",      "prompt": "You are a code reviewer. Focus on security, performance, and maintainability.",      "permission": {        "edit": "deny"      }    }  }}
```

---

### Markdown

You can also define agents using markdown files. Place them in:

-   Global: `~/.config/opencode/agents/`
-   Per-project: `.opencode/agents/`

~/.config/opencode/agents/review.md

```
---description: Reviews code for quality and best practicesmode: subagentmodel: anthropic/claude-sonnet-4-20250514temperature: 0.1permission:  edit: deny  bash: deny---
You are in code review mode. Focus on:
- Code quality and best practices- Potential bugs and edge cases- Performance implications- Security considerations
Provide constructive feedback without making direct changes.
```

The markdown file name becomes the agent name. For example, `review.md` creates a `review` agent.

### Markdown

你也可以使用 markdown 文件来定义代理。将它们放在：

-   全局：`~/.config/opencode/agents/`
-   按项目：`.opencode/agents/`

~/.config/opencode/agents/review.md

```
---description: Reviews code for quality and best practicesmode: subagentmodel: anthropic/claude-sonnet-4-20250514temperature: 0.1permission:  edit: deny  bash: deny---
You are in code review mode. Focus on:
- Code quality and best practices- Potential bugs and edge cases- Performance implications- Security considerations
Provide constructive feedback without making direct changes.
```

markdown 文件名即成为代理名。例如，`review.md` 会创建一个名为 `review` 的代理。

---

### Options

Let's look at these configuration options in detail.

### 选项

让我们详细了解这些配置选项。

---

### Description

Use the `description` option to provide a brief description of what the agent does and when to use it.

opencode.json

```
{  "agent": {    "review": {      "description": "Reviews code for best practices and potential issues"    }  }}
```

This is a **required** config option.

### Description

使用 `description` 选项提供关于代理做什么以及何时使用的简要说明。

opencode.json

```
{  "agent": {    "review": {      "description": "Reviews code for best practices and potential issues"    }  }}
```

这是一个**必填**配置项。

---

### Temperature

Control the randomness and creativity of the LLM's responses with the `temperature` config.

Lower values make responses more focused and deterministic, while higher values increase creativity and variability.

opencode.json

```
{  "agent": {    "plan": {      "temperature": 0.1    },    "creative": {      "temperature": 0.8    }  }}
```

Temperature values typically range from 0.0 to 1.0:

-   **0.0-0.2**: Very focused and deterministic responses, ideal for code analysis and planning
-   **0.3-0.5**: Balanced responses with some creativity, good for general development tasks
-   **0.6-1.0**: More creative and varied responses, useful for brainstorming and exploration

opencode.json

```
{  "agent": {    "analyze": {      "temperature": 0.1,      "prompt": "{file:./prompts/analysis.txt}"    },    "build": {      "temperature": 0.3    },    "brainstorm": {      "temperature": 0.7,      "prompt": "{file:./prompts/creative.txt}"    }  }}
```

If no temperature is specified, OpenCode uses model-specific defaults; typically 0 for most models, 0.55 for Qwen models.

### Temperature

使用 `temperature` 配置控制 LLM 响应的随机性和创造性。

较低的值使响应更聚焦、更确定，而较高的值则增加创造性和多变性。

opencode.json

```
{  "agent": {    "plan": {      "temperature": 0.1    },    "creative": {      "temperature": 0.8    }  }}
```

temperature 值通常在 0.0 到 1.0 之间：

-   **0.0-0.2**：非常聚焦且确定的响应，适合代码分析和规划
-   **0.3-0.5**：兼具一定创造性的均衡响应，适合一般开发任务
-   **0.6-1.0**：更具创造性和多样性的响应，适合头脑风暴和探索

opencode.json

```
{  "agent": {    "analyze": {      "temperature": 0.1,      "prompt": "{file:./prompts/analysis.txt}"    },    "build": {      "temperature": 0.3    },    "brainstorm": {      "temperature": 0.7,      "prompt": "{file:./prompts/creative.txt}"    }  }}
```

如果未指定 temperature，OpenCode 会使用模型相关的默认值；大多数模型通常为 0，Qwen 模型为 0.55。

---

### Max steps

Control the maximum number of agentic iterations an agent can perform before being forced to respond with text only. This allows users who wish to control costs to set a limit on agentic actions.

If this is not set, the agent will continue to iterate until the model chooses to stop or the user interrupts the session.

opencode.json

```
{  "agent": {    "quick-thinker": {      "description": "Fast reasoning with limited iterations",      "prompt": "You are a quick thinker. Solve problems with minimal steps.",      "steps": 5    }  }}
```

When the limit is reached, the agent receives a special system prompt instructing it to respond with a summarization of its work and recommended remaining tasks.

Caution

The legacy `maxSteps` field is deprecated. Use `steps` instead.

### Max steps

控制代理在被强制改为仅以文本响应之前可执行的最大代理式迭代次数。这让希望控制成本的用户可以对代理式操作设置上限。

如果未设置，代理将持续迭代，直到模型选择停止或用户中断会话。

opencode.json

```
{  "agent": {    "quick-thinker": {      "description": "Fast reasoning with limited iterations",      "prompt": "You are a quick thinker. Solve problems with minimal steps.",      "steps": 5    }  }}
```

当达到上限时，代理会收到一个特殊的系统提示词，指示它总结已完成的工作并给出推荐的剩余任务。

注意

旧字段 `maxSteps` 已弃用。请改用 `steps`。

---

### Disable

Set to `true` to disable the agent.

opencode.json

```
{  "agent": {    "review": {      "disable": true    }  }}
```

### Disable

设置为 `true` 以禁用该代理。

opencode.json

```
{  "agent": {    "review": {      "disable": true    }  }}
```

---

### Prompt

Specify a custom system prompt file for this agent with the `prompt` config. The prompt file should contain instructions specific to the agent's purpose.

opencode.json

```
{  "agent": {    "review": {      "prompt": "{file:./prompts/code-review.txt}"    }  }}
```

This path is relative to where the config file is located. So this works for both the global OpenCode config and the project specific config.

### Prompt

使用 `prompt` 配置为该代理指定一个自定义系统提示词文件。提示词文件应包含针对该代理用途的特定指令。

opencode.json

```
{  "agent": {    "review": {      "prompt": "{file:./prompts/code-review.txt}"    }  }}
```

此路径相对于配置文件所在位置。因此它同时适用于全局 OpenCode 配置和项目级配置。

---

### Model

Use the `model` config to override the model for this agent. Useful for using different models optimized for different tasks. For example, a faster model for planning, a more capable model for implementation.

Tip

If you don't specify a model, primary agents use the [model globally configured](/docs/config#models) while subagents will use the model of the primary agent that invoked the subagent.

opencode.json

```
{  "agent": {    "plan": {      "model": "anthropic/claude-haiku-4-20250514"    }  }}
```

The model ID in your OpenCode config uses the format `provider/model-id`. For example, if you're using [OpenCode Zen](/docs/zen), you would use `opencode/gpt-5.1-codex` for GPT 5.1 Codex.

### Model

使用 `model` 配置为该代理覆盖模型。适用于针对不同任务使用不同优化模型的情况。例如，规划用更快的模型，实现用更强的模型。

提示

如果不指定模型，主代理使用[全局配置的模型](/docs/config#models)，而子代理则使用调用它的那个主代理的模型。

opencode.json

```
{  "agent": {    "plan": {      "model": "anthropic/claude-haiku-4-20250514"    }  }}
```

OpenCode 配置中的模型 ID 采用 `provider/model-id` 格式。例如，如果你使用 [OpenCode Zen](/docs/zen)，则 GPT 5.1 Codex 对应 `opencode/gpt-5.1-codex`。

---

### Tools (deprecated)

`tools` is **deprecated**. Prefer the agent's [`permission`](#permissions) field for new configs, updates and more fine-grained control.

Allows you to control which tools are available in this agent. You can enable or disable specific tools by setting them to `true` or `false`. In an agent's `tools` config, `true` is equivalent to `{"*": "allow"}` permission and `false` is equivalent to `{"*": "deny"}` permission.

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "tools": {    "write": true,    "bash": true  },  "agent": {    "plan": {      "tools": {        "write": false,        "bash": false      }    }  }}
```

Note

The agent-specific config overrides the global config.

You can also use wildcards in legacy `tools` entries to control multiple tools at once. For example, to disable all tools from an MCP server:

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "readonly": {      "tools": {        "mymcp_*": false,        "write": false,        "edit": false      }    }  }}
```

[Learn more about tools](/docs/tools).

### Tools（已弃用）

`tools` **已弃用**。对于新配置、更新和更细粒度的控制，建议使用代理的 [`permission`](#permissions) 字段。

它用于控制该代理中可用的工具。可以通过将工具设置为 `true` 或 `false` 来启用或禁用特定工具。在代理的 `tools` 配置中，`true` 等价于 `{"*": "allow"}` 权限，`false` 等价于 `{"*": "deny"}` 权限。

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "tools": {    "write": true,    "bash": true  },  "agent": {    "plan": {      "tools": {        "write": false,        "bash": false      }    }  }}
```

注意

代理级配置会覆盖全局配置。

你还可以在旧的 `tools` 条目中使用通配符一次性控制多个工具。例如，要禁用某个 MCP 服务器的所有工具：

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "readonly": {      "tools": {        "mymcp_*": false,        "write": false,        "edit": false      }    }  }}
```

[了解更多关于 tools 的内容](/docs/tools)。

---

### Permissions

You can configure permissions to manage what actions an agent can take. Each permission key can be set to:

-   `"ask"` — Prompt for approval before running the tool
-   `"allow"` — Allow all operations without approval
-   `"deny"` — Disable the tool

The available permission keys are:

Key

Tools it gates

`read`

`read`

`edit`

`write`, `edit`, `apply_patch`

`glob`

`glob`

`grep`

`grep`

`list`

`list`

`bash`

`bash`

`task`

`task`

`external_directory`

Any tool that reads or writes files outside the project worktree

`todowrite`

`todowrite`, `todoread`

`webfetch`

`webfetch`

`websearch`

`websearch`

`lsp`

`lsp`

`skill`

`skill`

`question`

`question`

`doom_loop`

Recovery prompts when an agent appears stuck

`read`, `edit`, `glob`, `grep`, `list`, `bash`, `task`, `external_directory`, `lsp`, and `skill` accept either a shorthand action (`"allow" | "ask" | "deny"`) or an object of glob/pattern → action for fine-grained control. The remaining keys accept the shorthand action only.

Note

Permission keys are matched as wildcard patterns against the underlying tool name, so the same syntax works for built-ins, custom tools, and MCP tools — for example `"mymcp_*": "deny"` denies every tool from an MCP server, and `"mymcp_search": "ask"` targets a single one.

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "permission": {    "edit": "deny"  }}
```

You can override these permissions per agent.

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "permission": {    "edit": "deny"  },  "agent": {    "build": {      "permission": {        "edit": "ask"      }    }  }}
```

### Permissions

你可以配置权限来管理代理可执行的操作。每个权限键可设置为：

-   `"ask"` — 运行工具前提示批准
-   `"allow"` — 允许所有操作，无需批准
-   `"deny"` — 禁用该工具

可用的权限键如下：

键

控制的工具

`read`

`read`

`edit`

`write`、`edit`、`apply_patch`

`glob`

`glob`

`grep`

`grep`

`list`

`list`

`bash`

`bash`

`task`

`task`

`external_directory`

任何在项目工作树之外读写文件的工具

`todowrite`

`todowrite`、`todoread`

`webfetch`

`webfetch`

`websearch`

`websearch`

`lsp`

`lsp`

`skill`

`skill`

`question`

`question`

`doom_loop`

当代理看似卡住时触发的恢复提示词

`read`、`edit`、`glob`、`grep`、`list`、`bash`、`task`、`external_directory`、`lsp` 和 `skill` 既可接受简写动作（`"allow" | "ask" | "deny"`），也可接受一个 glob/模式 → 动作的对象以进行细粒度控制。其余键仅接受简写动作。

注意

权限键会作为通配符模式与底层工具名匹配，因此同一套语法对内置工具、自定义工具和 MCP 工具都适用——例如 `"mymcp_*": "deny"` 会拒绝某个 MCP 服务器的所有工具，而 `"mymcp_search": "ask"` 只针对单个工具。

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "permission": {    "edit": "deny"  }}
```

你可以按代理覆盖这些权限。

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "permission": {    "edit": "deny"  },  "agent": {    "build": {      "permission": {        "edit": "ask"      }    }  }}
```

You can also set permissions in Markdown agents.

~/.config/opencode/agents/review.md

```
---description: Code review without editsmode: subagentpermission:  edit: deny  bash:    "*": ask    "git diff": allow    "git log*": allow    "grep *": allow  webfetch: deny---
Only analyze code and suggest changes.
```

You can set permissions for specific bash commands.

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "permission": {        "bash": {          "git push": "ask",          "grep *": "allow"        }      }    }  }}
```

This can take a glob pattern.

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "permission": {        "bash": {          "git *": "ask"        }      }    }  }}
```

And you can also use the `*` wildcard to manage permissions for all commands. Since the last matching rule takes precedence, put the `*` wildcard first and specific rules after.

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "permission": {        "bash": {          "*": "ask",          "git status *": "allow"        }      }    }  }}
```

[Learn more about permissions](/docs/permissions).

你也可以在 Markdown 代理中设置权限。

~/.config/opencode/agents/review.md

```
---description: Code review without editsmode: subagentpermission:  edit: deny  bash:    "*": ask    "git diff": allow    "git log*": allow    "grep *": allow  webfetch: deny---
Only analyze code and suggest changes.
```

你可以为特定的 bash 命令设置权限。

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "permission": {        "bash": {          "git push": "ask",          "grep *": "allow"        }      }    }  }}
```

这里可使用 glob 模式。

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "permission": {        "bash": {          "git *": "ask"        }      }    }  }}
```

你也可以使用 `*` 通配符来管理所有命令的权限。由于最后匹配的规则优先，请把 `*` 通配符放在前面、具体规则放在后面。

opencode.json

```
{  "$schema": "https://opencode.ai/config.json",  "agent": {    "build": {      "permission": {        "bash": {          "*": "ask",          "git status *": "allow"        }      }    }  }}
```

[了解更多关于权限的内容](/docs/permissions)。

---

### Mode

Control the agent's mode with the `mode` config. The `mode` option is used to determine how the agent can be used.

opencode.json

```
{  "agent": {    "review": {      "mode": "subagent"    }  }}
```

The `mode` option can be set to `primary`, `subagent`, or `all`. If no `mode` is specified, it defaults to `all`.

### Mode

使用 `mode` 配置控制代理的模式。`mode` 选项用于决定代理的使用方式。

opencode.json

```
{  "agent": {    "review": {      "mode": "subagent"    }  }}
```

`mode` 选项可设置为 `primary`、`subagent` 或 `all`。若未指定 `mode`，则默认为 `all`。

---

### Hidden

Hide a subagent from the `@` autocomplete menu with `hidden: true`. Useful for internal subagents that should only be invoked programmatically by other agents via the Task tool.

opencode.json

```
{  "agent": {    "internal-helper": {      "mode": "subagent",      "hidden": true    }  }}
```

This only affects user visibility in the autocomplete menu. Hidden agents can still be invoked by the model via the Task tool if permissions allow.

Note

Only applies to `mode: subagent` agents.

### Hidden

使用 `hidden: true` 可将子代理从 `@` 自动补全菜单中隐藏。适用于只应由其他代理通过 Task 工具以编程方式调用的内部子代理。

opencode.json

```
{  "agent": {    "internal-helper": {      "mode": "subagent",      "hidden": true    }  }}
```

这仅影响用户在自动补全菜单中的可见性。若权限允许，隐藏的代理仍可由模型通过 Task 工具调用。

注意

仅适用于 `mode: subagent` 的代理。

---

### Task permissions

Control which subagents an agent can invoke via the Task tool with `permission.task`. Uses glob patterns for flexible matching.

opencode.json

```
{  "agent": {    "orchestrator": {      "mode": "primary",      "permission": {        "task": {          "*": "deny",          "orchestrator-*": "allow",          "code-reviewer": "ask"        }      }    }  }}
```

When set to `deny`, the subagent is removed from the Task tool description entirely, so the model won't attempt to invoke it.

Tip

Rules are evaluated in order, and the **last matching rule wins**. In the example above, `orchestrator-planner` matches both `*` (deny) and `orchestrator-*` (allow), but since `orchestrator-*` comes after `*`, the result is `allow`.

Tip

Users can always invoke any subagent directly via the `@` autocomplete menu, even if the agent's task permissions would deny it.

### Task permissions

使用 `permission.task` 控制代理可通过 Task 工具调用哪些子代理。使用 glob 模式进行灵活匹配。

opencode.json

```
{  "agent": {    "orchestrator": {      "mode": "primary",      "permission": {        "task": {          "*": "deny",          "orchestrator-*": "allow",          "code-reviewer": "ask"        }      }    }  }}
```

当设置为 `deny` 时，该子代理会从 Task 工具描述中完全移除，因此模型不会尝试调用它。

提示

规则按顺序求值，**最后匹配的规则生效**。在上面的示例中，`orchestrator-planner` 同时匹配 `*`（deny）和 `orchestrator-*`（allow），但由于 `orchestrator-*` 在 `*` 之后，结果为 `allow`。

提示

用户始终可以通过 `@` 自动补全菜单直接调用任何子代理，即使该代理的 task 权限会拒绝它。

---

### Color

Customize the agent's visual appearance in the UI with the `color` option. This affects how the agent appears in the interface.

Use a valid hex color (e.g., `#FF5733`) or theme color: `primary`, `secondary`, `accent`, `success`, `warning`, `error`, `info`.

opencode.json

```
{  "agent": {    "creative": {      "color": "#ff6b6b"    },    "code-reviewer": {      "color": "accent"    }  }}
```

### Color

使用 `color` 选项自定义代理在 UI 中的视觉外观。这会影响代理在界面中的呈现方式。

使用有效的十六进制颜色（如 `#FF5733`）或主题色：`primary`、`secondary`、`accent`、`success`、`warning`、`error`、`info`。

opencode.json

```
{  "agent": {    "creative": {      "color": "#ff6b6b"    },    "code-reviewer": {      "color": "accent"    }  }}
```

---

### Top P

Control response diversity with the `top_p` option. Alternative to temperature for controlling randomness.

opencode.json

```
{  "agent": {    "brainstorm": {      "top_p": 0.9    }  }}
```

Values range from 0.0 to 1.0. Lower values are more focused, higher values more diverse.

### Top P

使用 `top_p` 选项控制响应的多样性。它是 temperature 之外控制随机性的另一种方式。

opencode.json

```
{  "agent": {    "brainstorm": {      "top_p": 0.9    }  }}
```

取值范围为 0.0 到 1.0。值越低越聚焦，值越高越多样。

---

### Additional

Any other options you specify in your agent configuration will be **passed through directly** to the provider as model options. This allows you to use provider-specific features and parameters.

For example, with OpenAI's reasoning models, you can control the reasoning effort:

opencode.json

```
{  "agent": {    "deep-thinker": {      "description": "Agent that uses high reasoning effort for complex problems",      "model": "openai/gpt-5",      "reasoningEffort": "high",      "textVerbosity": "low"    }  }}
```

These additional options are model and provider-specific. Check your provider's documentation for available parameters.

Tip

Run `opencode models` to see a list of the available models.

### Additional

你在代理配置中指定的任何其他选项都将**直接透传**给 provider 作为模型选项。这让你可以使用 provider 特有的功能和参数。

例如，对于 OpenAI 的推理模型，你可以控制推理强度：

opencode.json

```
{  "agent": {    "deep-thinker": {      "description": "Agent that uses high reasoning effort for complex problems",      "model": "openai/gpt-5",      "reasoningEffort": "high",      "textVerbosity": "low"    }  }}
```

这些附加选项与模型和 provider 相关。请查阅你的 provider 文档以了解可用参数。

提示

运行 `opencode models` 可查看可用模型列表。

---

### Create agents

You can create new agents using the following command:

Terminal window

```
opencode agent create
```

This interactive command will:

1.  Ask where to save the agent; global or project-specific.
2.  Description of what the agent should do.
3.  Generate an appropriate system prompt and identifier.
4.  Let you select which permissions the agent should be allowed (anything you don't select is denied).
5.  Finally, create a markdown file with the agent configuration.

### Create agents

你可以使用以下命令创建新代理：

Terminal window

```
opencode agent create
```

这个交互式命令会：

1.  询问将代理保存到哪里：全局或项目级。
2.  询问代理应做什么的描述。
3.  生成合适的系统提示词和标识符。
4.  让你选择代理应被允许哪些权限（未选择的将被拒绝）。
5.  最后，创建一个包含代理配置的 markdown 文件。

---

### Use cases

Here are some common use cases for different agents.

-   **Build agent**: Full development work with all tools enabled
-   **Plan agent**: Analysis and planning without making changes
-   **Review agent**: Code review with read-only access plus documentation tools
-   **Debug agent**: Focused on investigation with bash and read tools enabled
-   **Docs agent**: Documentation writing with file operations but no system commands

### Use cases

下面是一些不同代理的常见用例。

-   **Build 代理**：启用所有工具的完整开发工作
-   **Plan 代理**：只做分析和规划，不做改动
-   **Review 代理**：以只读权限进行代码审查（Code Review），外加文档工具
-   **Debug 代理**：以调查为重心，启用 bash 和读取工具
-   **Docs 代理**：文档编写，可进行文件操作但不执行系统命令

---

### Examples

Here are some example agents you might find useful.

Tip

Do you have an agent you'd like to share? [Submit a PR](https://github.com/anomalyco/opencode).

### Examples

下面是一些你可能会觉得有用的示例代理。

提示

你有想要分享的代理吗？[提交一个拉取请求（Pull Request）](https://github.com/anomalyco/opencode)。

---

### Documentation agent

~/.config/opencode/agents/docs-writer.md

```
---description: Writes and maintains project documentationmode: subagentpermission:  bash: deny---
You are a technical writer. Create clear, comprehensive documentation.
Focus on:
- Clear explanations- Proper structure- Code examples- User-friendly language
```

### Documentation agent

~/.config/opencode/agents/docs-writer.md

```
---description: Writes and maintains project documentationmode: subagentpermission:  bash: deny---
You are a technical writer. Create clear, comprehensive documentation.
Focus on:
- Clear explanations- Proper structure- Code examples- User-friendly language
```

---

### Security auditor

~/.config/opencode/agents/security-auditor.md

```
---description: Performs security audits and identifies vulnerabilitiesmode: subagentpermission:  edit: deny---
You are a security expert. Focus on identifying potential security issues.
Look for:
- Input validation vulnerabilities- Authentication and authorization flaws- Data exposure risks- Dependency vulnerabilities- Configuration security issues
```

### Security auditor

~/.config/opencode/agents/security-auditor.md

```
---description: Performs security audits and identifies vulnerabilitiesmode: subagentpermission:  edit: deny---
You are a security expert. Focus on identifying potential security issues.
Look for:
- Input validation vulnerabilities- Authentication and authorization flaws- Data exposure risks- Dependency vulnerabilities- Configuration security issues
```


---

## OpenCode - Configuration（配置）

原文链接：https://opencode.ai/docs/config

### Config

Using the OpenCode JSON config.

You can configure OpenCode using a JSON config file.

使用 OpenCode 的 JSON 配置。

你可以通过 JSON 配置文件来配置 OpenCode。

### Format

OpenCode supports both **JSON** and **JSONC** (JSON with Comments) formats.

OpenCode 同时支持 **JSON** 和 **JSONC**（带注释的 JSON）两种格式。

opencode.jsonc

```
{
  "$schema": "https://opencode.ai/config.json",
  "model": "anthropic/claude-sonnet-4-5",
  "autoupdate": true,
  "server": {
    "port": 4096,
  },
}
```

### Locations

You can place your config in a couple of different locations and they have a different order of precedence.

Note

Configuration files are **merged together**, not replaced.

Configuration files are merged together, not replaced. Settings from the following config locations are combined. Later configs override earlier ones only for conflicting keys. Non-conflicting settings from all configs are preserved.

For example, if your global config sets `autoupdate: true` and your project config sets `model: "anthropic/claude-sonnet-4-5"`, the final configuration will include both settings.

### 位置（Locations）

你可以将配置放在几个不同的位置，它们之间有不同的优先级顺序。

注意

配置文件是**合并在一起**的，而非互相替换。

配置文件会合并在一起，而不是被替换。以下各配置位置中的设置会被组合起来。后加载的配置仅在键冲突时覆盖先前的配置，所有配置中不冲突的设置都会被保留。

例如，如果你的全局配置设置了 `autoupdate: true`，而项目配置设置了 `model: "anthropic/claude-sonnet-4-5"`，最终的配置将同时包含这两项设置。

### Precedence order

Config sources are loaded in this order (later sources override earlier ones):

1. **Remote config** (from `.well-known/opencode`) - organizational defaults
2. **Global config** (`~/.config/opencode/opencode.json`) - user preferences
3. **Custom config** (`OPENCODE_CONFIG` env var) - custom overrides
4. **Project config** (`opencode.json` in project) - project-specific settings
5. **`.opencode` directories** - agents, commands, plugins
6. **Inline config** (`OPENCODE_CONFIG_CONTENT` env var) - runtime overrides
7. **Managed config files** (`/Library/Application Support/opencode/` on macOS) - admin-controlled
8. **macOS managed preferences** (`.mobileconfig` via MDM) - highest priority, not user-overridable

This means project configs can override global defaults, and global configs can override remote organizational defaults. Managed settings override everything.

Note

The `.opencode` and `~/.config/opencode` directories use **plural names** for subdirectories: `agents/`, `commands/`, `modes/`, `plugins/`, `skills/`, `tools/`, and `themes/`. Singular names (e.g., `agent/`) are also supported for backwards compatibility.

### 优先级顺序（Precedence order）

配置源按以下顺序加载（后加载的覆盖先加载的）：

1. **Remote config**（远程配置）(来自 `.well-known/opencode`) - 组织级默认值
2. **Global config**（全局配置）(`~/.config/opencode/opencode.json`) - 用户偏好
3. **Custom config**（自定义配置）(`OPENCODE_CONFIG` 环境变量) - 自定义覆盖
4. **Project config**（项目配置）(项目中的 `opencode.json`) - 项目特定设置
5. **`.opencode` directories**（`.opencode` 目录）- agents、commands、plugins
6. **Inline config**（内联配置）(`OPENCODE_CONFIG_CONTENT` 环境变量) - 运行时覆盖
7. **Managed config files**（托管配置文件）(macOS 上的 `/Library/Application Support/opencode/`) - 管理员控制
8. **macOS managed preferences**（macOS 托管偏好设置）(通过 MDM 的 `.mobileconfig`) - 最高优先级，用户无法覆盖

这意味着项目配置可以覆盖全局默认值，而全局配置可以覆盖远程组织默认值。托管设置（Managed settings）覆盖一切。

注意

`.opencode` 和 `~/.config/opencode` 目录的子目录使用**复数名称**：`agents/`、`commands/`、`modes/`、`plugins/`、`skills/`、`tools/` 和 `themes/`。单数名称（例如 `agent/`）出于向后兼容也被支持。

### Remote

Organizations can provide default configuration via the `.well-known/opencode` endpoint. This is fetched automatically when you authenticate with a provider that supports it.

Remote config is loaded first, serving as the base layer. All other config sources (global, project) can override these defaults.

For example, if your organization provides MCP servers that are disabled by default:

Remote config from .well-known/opencode

```
{
  "mcp": {
    "jira": {
      "type": "remote",
      "url": "https://jira.example.com/mcp",
      "enabled": false
    }
  }
}
```

You can enable specific servers in your local config:

opencode.json

```
{
  "mcp": {
    "jira": {
      "type": "remote",
      "url": "https://jira.example.com/mcp",
      "enabled": true
    }
  }
}
```

### Remote（远程配置）

组织可以通过 `.well-known/opencode` 端点提供默认配置。当你通过支持该功能的 provider（提供者）进行身份验证时，该配置会被自动获取。

Remote config 最先加载，作为基础层。所有其他配置源（全局、项目）都可以覆盖这些默认值。

例如，如果你的组织提供了默认禁用的 MCP servers（MCP 服务器）：

来自 .well-known/opencode 的 Remote config

```
{
  "mcp": {
    "jira": {
      "type": "remote",
      "url": "https://jira.example.com/mcp",
      "enabled": false
    }
  }
}
```

你可以在本地配置中启用特定的服务器：

opencode.json

```
{
  "mcp": {
    "jira": {
      "type": "remote",
      "url": "https://jira.example.com/mcp",
      "enabled": true
    }
  }
}
```

### Global

Place your global OpenCode config in `~/.config/opencode/opencode.json`. Use global config for user-wide server/runtime preferences like providers, models, and permissions.

For TUI-specific settings, use `~/.config/opencode/tui.json`.

Global config overrides remote organizational defaults.

### Global（全局配置）

将你的全局 OpenCode 配置放在 `~/.config/opencode/opencode.json`。全局配置用于用户范围的服务器/运行时偏好，例如 providers、models 和 permissions。

对于 TUI（终端用户界面）特定的设置，使用 `~/.config/opencode/tui.json`。

Global config 会覆盖远程组织默认值。

### Per project

Add `opencode.json` in your project root. Project config has the highest precedence among standard config files - it overrides both global and remote configs.

For project-specific TUI settings, add `tui.json` alongside it.

Tip

Place project specific config in the root of your project.

When OpenCode starts up, it first looks for a config file in the current directory, then traverses up to the nearest Git directory.

This is also safe to be checked into Git and uses the same schema as the global one.

### Per project（按项目配置）

在你的项目根目录添加 `opencode.json`。在标准配置文件中，项目配置具有最高优先级——它会同时覆盖全局配置和远程配置。

对于项目特定的 TUI 设置，在旁边添加 `tui.json`。

提示

将项目特定的配置放在项目根目录。

当 OpenCode 启动时，它会先在当前目录查找配置文件，然后向上遍历到最近的 Git 目录。

该配置文件也可以安全地提交到 Git 中，并使用与全局配置相同的 schema。

### Custom path

Specify a custom config file path using the `OPENCODE_CONFIG` environment variable.

Terminal window

```
export OPENCODE_CONFIG=/path/to/my/custom-config.json
opencode run "Hello world"
```

Custom config is loaded between global and project configs in the precedence order.

### Custom path（自定义路径）

使用 `OPENCODE_CONFIG` 环境变量指定自定义配置文件路径。

终端窗口（此处展示一段终端命令演示）

```
export OPENCODE_CONFIG=/path/to/my/custom-config.json
opencode run "Hello world"
```

Custom config 在优先级顺序中于全局配置和项目配置之间加载。

### Custom directory

Specify a custom config directory using the `OPENCODE_CONFIG_DIR` environment variable. This directory will be searched for agents, commands, modes, and plugins just like the standard `.opencode` directory, and should follow the same structure.

Terminal window

```
export OPENCODE_CONFIG_DIR=/path/to/my/config-directory
opencode run "Hello world"
```

The custom directory is loaded after the global config and `.opencode` directories, so it **can override** their settings.

### Custom directory（自定义目录）

使用 `OPENCODE_CONFIG_DIR` 环境变量指定自定义配置目录。该目录将像标准的 `.opencode` 目录一样被搜索 agents、commands、modes 和 plugins，并应遵循相同的结构。

终端窗口（此处展示一段终端命令演示）

```
export OPENCODE_CONFIG_DIR=/path/to/my/config-directory
opencode run "Hello world"
```

自定义目录在全局配置和 `.opencode` 目录之后加载，因此它**可以覆盖**这些目录的设置。

### Managed settings

Organizations can enforce configuration that users cannot override. Managed settings are loaded at the highest priority tier.

### Managed settings（托管设置）

组织可以强制执行用户无法覆盖的配置。Managed settings 在最高优先级层加载。

#### File-based

Drop an `opencode.json` or `opencode.jsonc` file in the system managed config directory:

| Platform | Path |
| --- | --- |
| macOS | `/Library/Application Support/opencode/` |
| Linux | `/etc/opencode/` |
| Windows | `%ProgramData%\opencode` |

These directories require admin/root access to write, so users cannot modify them.

#### 基于文件（File-based）

在系统托管配置目录中放入一个 `opencode.json` 或 `opencode.jsonc` 文件：

| 平台 | 路径 |
| --- | --- |
| macOS | `/Library/Application Support/opencode/` |
| Linux | `/etc/opencode/` |
| Windows | `%ProgramData%\opencode` |

这些目录需要管理员/root 权限才能写入，因此用户无法修改它们。

#### macOS managed preferences

On macOS, OpenCode reads managed preferences from the `ai.opencode.managed` preference domain. Deploy a `.mobileconfig` via MDM (Jamf, Kandji, FleetDM) and the settings are enforced automatically.

OpenCode checks these paths:

1. `/Library/Managed Preferences/<user>/ai.opencode.managed.plist`
2. `/Library/Managed Preferences/ai.opencode.managed.plist`

The plist keys map directly to `opencode.json` fields. MDM metadata keys (`PayloadUUID`, `PayloadType`, etc.) are stripped automatically.

#### macOS 托管偏好设置（macOS managed preferences）

在 macOS 上，OpenCode 从 `ai.opencode.managed` 偏好设置域读取托管偏好。通过 MDM（移动设备管理）(如 Jamf、Kandji、FleetDM) 部署 `.mobileconfig`，设置会被自动强制执行。

OpenCode 会检查以下路径：

1. `/Library/Managed Preferences/<user>/ai.opencode.managed.plist`
2. `/Library/Managed Preferences/ai.opencode.managed.plist`

plist 中的键直接映射到 `opencode.json` 的字段。MDM 元数据键（`PayloadUUID`、`PayloadType` 等）会被自动剥离。

**Creating a `.mobileconfig`**

Use the `ai.opencode.managed` PayloadType. The OpenCode config keys go directly in the payload dict:

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>PayloadContent</key>
  <array>
    <dict>
      <key>PayloadType</key>
      <string>ai.opencode.managed</string>
      <key>PayloadIdentifier</key>
      <string>com.example.opencode.config</string>
      <key>PayloadUUID</key>
      <string>GENERATE-YOUR-OWN-UUID</string>
      <key>PayloadVersion</key>
      <integer>1</integer>
      <key>share</key>
      <string>disabled</string>
      <key>server</key>
      <dict>
        <key>hostname</key>
        <string>127.0.0.1</string>
      </dict>
      <key>permission</key>
      <dict>
        <key>*</key>
        <string>ask</string>
        <key>bash</key>
        <dict>
          <key>*</key>
          <string>ask</string>
          <key>rm -rf *</key>
          <string>deny</string>
        </dict>
      </dict>
    </dict>
  </array>
  <key>PayloadType</key>
  <string>Configuration</string>
  <key>PayloadIdentifier</key>
  <string>com.example.opencode</string>
  <key>PayloadUUID</key>
  <string>GENERATE-YOUR-OWN-UUID</string>
  <key>PayloadVersion</key>
  <integer>1</integer>
</dict>
</plist>
```

Generate unique UUIDs with `uuidgen`. Customize the settings to match your organization's requirements.

**创建 `.mobileconfig`**

使用 `ai.opencode.managed` 作为 PayloadType。OpenCode 的配置键直接放在 payload dict 中：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN"
  "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>PayloadContent</key>
  <array>
    <dict>
      <key>PayloadType</key>
      <string>ai.opencode.managed</string>
      <key>PayloadIdentifier</key>
      <string>com.example.opencode.config</string>
      <key>PayloadUUID</key>
      <string>GENERATE-YOUR-OWN-UUID</string>
      <key>PayloadVersion</key>
      <integer>1</integer>
      <key>share</key>
      <string>disabled</string>
      <key>server</key>
      <dict>
        <key>hostname</key>
        <string>127.0.0.1</string>
      </dict>
      <key>permission</key>
      <dict>
        <key>*</key>
        <string>ask</string>
        <key>bash</key>
        <dict>
          <key>*</key>
          <string>ask</string>
          <key>rm -rf *</key>
          <string>deny</string>
        </dict>
      </dict>
    </dict>
  </array>
  <key>PayloadType</key>
  <string>Configuration</string>
  <key>PayloadIdentifier</key>
  <string>com.example.opencode</string>
  <key>PayloadUUID</key>
  <string>GENERATE-YOUR-OWN-UUID</string>
  <key>PayloadVersion</key>
  <integer>1</integer>
</dict>
</plist>
```

使用 `uuidgen` 生成唯一的 UUID。根据你组织的需求自定义这些设置。

**Deploying via MDM**

- **Jamf Pro:** Computers > Configuration Profiles > Upload > scope to target devices or smart groups
- **FleetDM:** Add the `.mobileconfig` to your gitops repo under `mdm.macos_settings.custom_settings` and run `fleetctl apply`

**Verifying on a device**

Double-click the `.mobileconfig` to install locally for testing (shows in System Settings > Privacy & Security > Profiles), then run:

Terminal window

```
opencode debug config
```

All managed preference keys appear in the resolved config and cannot be overridden by user or project configuration.

**通过 MDM 部署（Deploying via MDM）**

- **Jamf Pro：** Computers > Configuration Profiles > Upload > 作用域设置为目标设备或智能分组
- **FleetDM：** 将 `.mobileconfig` 添加到你的 gitops 仓库的 `mdm.macos_settings.custom_settings` 下，然后运行 `fleetctl apply`

**在设备上验证（Verifying on a device）**

双击 `.mobileconfig` 以在本地安装进行测试（会显示在“系统设置”>“隐私与安全性”>“描述文件”中），然后运行：

终端窗口（此处展示一段终端命令演示）

```
opencode debug config
```

所有托管偏好键都会出现在解析后的配置中，并且无法被用户或项目配置覆盖。

### Schema

The server/runtime config schema is defined in [**`opencode.ai/config.json`**](https://opencode.ai/config.json).

TUI config uses [**`opencode.ai/tui.json`**](https://opencode.ai/tui.json).

Your editor should be able to validate and autocomplete based on the schema.

### Schema（模式定义）

服务器/运行时配置的 schema 定义在 [**`opencode.ai/config.json`**](https://opencode.ai/config.json) 中。

TUI 配置使用 [**`opencode.ai/tui.json`**](https://opencode.ai/tui.json)。

你的编辑器应该能够基于该 schema 进行验证和自动补全。

### TUI

Use a dedicated `tui.json` (or `tui.jsonc`) file for TUI-specific settings.

tui.json

```
{
  "$schema": "https://opencode.ai/tui.json",
  "scroll_speed": 3,
  "scroll_acceleration": {
    "enabled": true
  },
  "diff_style": "auto",
  "mouse": true,
  "attention": {
    "enabled": true,
    "notifications": true,
    "sound": true,
    "volume": 0.4
  }
}
```

Use `OPENCODE_TUI_CONFIG` to point to a custom TUI config file.

Set `attention.enabled` to turn on TUI desktop notifications and sounds. See [TUI attention](/docs/tui#attention).

Legacy `theme`, `keybinds`, and `tui` keys in `opencode.json` are deprecated and automatically migrated when possible.

### TUI

为 TUI 特定的设置使用专用的 `tui.json`（或 `tui.jsonc`）文件。

tui.json

```
{
  "$schema": "https://opencode.ai/tui.json",
  "scroll_speed": 3,
  "scroll_acceleration": {
    "enabled": true
  },
  "diff_style": "auto",
  "mouse": true,
  "attention": {
    "enabled": true,
    "notifications": true,
    "sound": true,
    "volume": 0.4
  }
}
```

使用 `OPENCODE_TUI_CONFIG` 指向自定义的 TUI 配置文件。

设置 `attention.enabled` 可开启 TUI 桌面通知和声音。参见 [TUI attention](/docs/tui#attention)。

`opencode.json` 中传统的 `theme`、`keybinds` 和 `tui` 键已被弃用，在可能的情况下会自动迁移。

### Server

You can configure server settings for the `opencode serve` and `opencode web` commands through the `server` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "server": {
    "port": 4096,
    "hostname": "0.0.0.0",
    "mdns": true,
    "mdnsDomain": "myproject.local",
    "cors": ["http://localhost:5173"]
  }
}
```

Available options:

- `port` - Port to listen on.
- `hostname` - Hostname to listen on. When `mdns` is enabled and no hostname is set, defaults to `0.0.0.0`.
- `mdns` - Enable mDNS service discovery. This allows other devices on the network to discover your OpenCode server.
- `mdnsDomain` - Custom domain name for mDNS service. Defaults to `opencode.local`. Useful for running multiple instances on the same network.
- `cors` - Additional origins to allow for CORS when using the HTTP server from a browser-based client. Values must be full origins (scheme + host + optional port), eg `https://app.example.com`.

[Learn more about the server here](/docs/server).

### Server（服务器）

你可以通过 `server` 选项为 `opencode serve` 和 `opencode web` 命令配置服务器设置。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "server": {
    "port": 4096,
    "hostname": "0.0.0.0",
    "mdns": true,
    "mdnsDomain": "myproject.local",
    "cors": ["http://localhost:5173"]
  }
}
```

可用选项：

- `port` - 监听的端口。
- `hostname` - 监听的主机名。当启用了 `mdns` 且未设置 hostname 时，默认为 `0.0.0.0`。
- `mdns` - 启用 mDNS 服务发现。这允许网络上的其他设备发现你的 OpenCode 服务器。
- `mdnsDomain` - mDNS 服务的自定义域名。默认为 `opencode.local`。适用于在同一网络上运行多个实例的场景。
- `cors` - 当从基于浏览器的客户端使用 HTTP 服务器时，允许用于 CORS（跨源资源共享）的额外源。值必须是完整的源（协议 + 主机 + 可选端口），例如 `https://app.example.com`。

[在此了解更多关于服务器的信息](/docs/server)。

### Shell

You can configure the shell used for the interactive terminal using the `shell` option. Compatible shells are also used for agent tool calls.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "shell": "pwsh"
}
```

If not specified, OpenCode will automatically discover and use a sensible default based on your operating system (e.g. `pwsh` or `cmd.exe` on Windows, `/bin/zsh` or `/bin/bash` on macOS/Linux). You can provide an absolute path or a short name.

### Shell（命令行解释器）

你可以使用 `shell` 选项配置交互式终端所使用的 shell。兼容的 shell 也会用于 agent（智能体）的工具调用。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "shell": "pwsh"
}
```

如果未指定，OpenCode 会根据你的操作系统自动发现并使用一个合理的默认值（例如 Windows 上的 `pwsh` 或 `cmd.exe`，macOS/Linux 上的 `/bin/zsh` 或 `/bin/bash`）。你可以提供绝对路径或简短名称。

### Tools

You can manage the tools an LLM can use through the `tools` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "tools": {
    "write": false,
    "bash": false
  }
}
```

[Learn more about tools here](/docs/tools).

### Tools（工具）

你可以通过 `tools` 选项管理 LLM（大语言模型）可以使用的工具。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "tools": {
    "write": false,
    "bash": false
  }
}
```

[在此了解更多关于工具的信息](/docs/tools)。

### Models

You can configure the providers and models you want to use in your OpenCode config through the `provider`, `model` and `small_model` options.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {},
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5"
}
```

The `small_model` option configures a separate model for lightweight tasks like title generation. By default, OpenCode tries to use a cheaper model if one is available from your provider, otherwise it falls back to your main model.

### Models（模型）

你可以在 OpenCode 配置中通过 `provider`、`model` 和 `small_model` 选项配置你想要使用的 providers 和 models。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {},
  "model": "anthropic/claude-sonnet-4-5",
  "small_model": "anthropic/claude-haiku-4-5"
}
```

`small_model` 选项用于为标题生成等轻量级任务配置一个单独的模型。默认情况下，如果你的 provider 提供了更便宜的模型，OpenCode 会尝试使用它，否则会回退到你的主模型。

Provider options can include `timeout`, `chunkTimeout`, and `setCacheKey`:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "anthropic": {
      "options": {
        "timeout": 600000,
        "chunkTimeout": 30000,
        "setCacheKey": true
      }
    }
  }
}
```

- `timeout` - Request timeout in milliseconds (default: 300000). Set to `false` to disable.
- `chunkTimeout` - Timeout in milliseconds between streamed response chunks. If no chunk arrives in time, the request is aborted.
- `setCacheKey` - Ensure a cache key is always set for designated provider.

You can also configure [local models](/docs/models#local). [Learn more](/docs/models).

Provider 选项可以包含 `timeout`、`chunkTimeout` 和 `setCacheKey`：

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "anthropic": {
      "options": {
        "timeout": 600000,
        "chunkTimeout": 30000,
        "setCacheKey": true
      }
    }
  }
}
```

- `timeout` - 请求超时时间，单位为毫秒（默认：300000）。设置为 `false` 可禁用。
- `chunkTimeout` - 流式响应数据块之间的超时时间，单位为毫秒。如果在指定时间内没有数据块到达，请求将被中止。
- `setCacheKey` - 确保为指定的 provider 始终设置缓存键。

你也可以配置[本地模型](/docs/models#local)。[了解更多](/docs/models)。

### Policies

Use the `experimental.policies` option to allow or deny OpenCode actions on configured resources. Currently, policies can control which providers OpenCode may use.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "experimental": {
    "policies": [
      {
        "effect": "deny",
        "action": "provider.use",
        "resource": "openai"
      }
    ]
  }
}
```

[Learn more about policies here](/docs/policies).

### Policies（策略）

使用 `experimental.policies` 选项来允许或拒绝 OpenCode 对已配置资源执行的操作。目前，policies 可以控制 OpenCode 可以使用哪些 providers。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "experimental": {
    "policies": [
      {
        "effect": "deny",
        "action": "provider.use",
        "resource": "openai"
      }
    ]
  }
}
```

[在此了解更多关于 policies 的信息](/docs/policies)。

### Image attachments

OpenCode normalizes image attachments before sending them to the model. By default, images are resized when they exceed `2000x2000` pixels or `5242880` base64 bytes.

Configure image attachment limits with the `attachment.image` option:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "attachment": {
    "image": {
      "auto_resize": true,
      "max_width": 2000,
      "max_height": 2000,
      "max_base64_bytes": 5242880
    }
  }
}
```

- `auto_resize` - Resize images that exceed the configured limits before provider requests. Set to `false` to reject oversized images instead.
- `max_width` - Maximum image width in pixels before resizing or rejection.
- `max_height` - Maximum image height in pixels before resizing or rejection.
- `max_base64_bytes` - Maximum encoded image payload size. This is the base64 payload size, not the original file size.

If an image still cannot fit after resizing, OpenCode omits oversized tool-result images or fails oversized user-provided images with an image size error.

### Image attachments（图片附件）

OpenCode 在将图片附件发送给模型之前会对其进行规范化处理。默认情况下，当图片超过 `2000x2000` 像素或 `5242880` 个 base64 字节时，会进行缩放。

使用 `attachment.image` 选项配置图片附件的限制：

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "attachment": {
    "image": {
      "auto_resize": true,
      "max_width": 2000,
      "max_height": 2000,
      "max_base64_bytes": 5242880
    }
  }
}
```

- `auto_resize` - 在 provider 请求之前，对超过配置限制的图片进行缩放。设置为 `false` 则改为拒绝过大的图片。
- `max_width` - 缩放或拒绝前的最大图片宽度，单位为像素。
- `max_height` - 缩放或拒绝前的最大图片高度，单位为像素。
- `max_base64_bytes` - 最大编码图片载荷大小。这是 base64 载荷的大小，而非原始文件大小。

如果图片在缩放后仍然无法满足大小要求，OpenCode 会省略过大的工具结果图片，或对过大的用户提供的图片报出图片大小错误。

#### Provider-Specific Options

Some providers support additional configuration options beyond the generic `timeout` and `apiKey` settings.

#### Provider 特定选项（Provider-Specific Options）

某些 providers 支持除通用的 `timeout` 和 `apiKey` 设置之外的额外配置选项。

##### Amazon Bedrock

Amazon Bedrock supports AWS-specific configuration:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "amazon-bedrock": {
      "options": {
        "region": "us-east-1",
        "profile": "my-aws-profile",
        "endpoint": "https://bedrock-runtime.us-east-1.vpce-xxxxx.amazonaws.com"
      }
    }
  }
}
```

- `region` - AWS region for Bedrock (defaults to `AWS_REGION` env var or `us-east-1`)
- `profile` - AWS named profile from `~/.aws/credentials` (defaults to `AWS_PROFILE` env var)
- `endpoint` - Custom endpoint URL for VPC endpoints. This is an alias for the generic `baseURL` option using AWS-specific terminology. If both are specified, `endpoint` takes precedence.

Note

Bearer tokens (`AWS_BEARER_TOKEN_BEDROCK` or `/connect`) take precedence over profile-based authentication. See [authentication precedence](/docs/providers#authentication-precedence) for details.

[Learn more about Amazon Bedrock configuration](/docs/providers#amazon-bedrock).

##### Amazon Bedrock

Amazon Bedrock 支持 AWS 特定的配置：

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "amazon-bedrock": {
      "options": {
        "region": "us-east-1",
        "profile": "my-aws-profile",
        "endpoint": "https://bedrock-runtime.us-east-1.vpce-xxxxx.amazonaws.com"
      }
    }
  }
}
```

- `region` - Bedrock 的 AWS 区域（默认为 `AWS_REGION` 环境变量或 `us-east-1`）
- `profile` - 来自 `~/.aws/credentials` 的 AWS 命名配置文件（默认为 `AWS_PROFILE` 环境变量）
- `endpoint` - 用于 VPC 端点的自定义端点 URL。这是通用 `baseURL` 选项使用 AWS 特定术语的别名。如果两者都指定了，`endpoint` 优先。

注意

Bearer tokens（持有者令牌）(`AWS_BEARER_TOKEN_BEDROCK` 或 `/connect`) 优先于基于 profile 的身份验证。详情参见[身份验证优先级](/docs/providers#authentication-precedence)。

[在此了解更多关于 Amazon Bedrock 配置的信息](/docs/providers#amazon-bedrock)。

### Themes

Set your UI theme in `tui.json`.

tui.json

```
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "tokyonight"
}
```

[Learn more here](/docs/themes).

### Themes（主题）

在 `tui.json` 中设置你的 UI 主题。

tui.json

```
{
  "$schema": "https://opencode.ai/tui.json",
  "theme": "tokyonight"
}
```

[在此了解更多](/docs/themes)。

### Agents

You can configure specialized agents for specific tasks through the `agent` option.

opencode.jsonc

```
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "code-reviewer": {
      "description": "Reviews code for best practices and potential issues",
      "model": "anthropic/claude-sonnet-4-5",
      "prompt": "You are a code reviewer. Focus on security, performance, and maintainability.",
      "tools": {
        // Disable file modification tools for review-only agent
        "write": false,
        "edit": false,
      },
    },
  },
}
```

You can also define agents using markdown files in `~/.config/opencode/agents/` or `.opencode/agents/`. [Learn more here](/docs/agents).

### Agents（智能体）

你可以通过 `agent` 选项为特定任务配置专门的 agents（智能体/专门执行特定任务的 AI 实体）。

opencode.jsonc

```
{
  "$schema": "https://opencode.ai/config.json",
  "agent": {
    "code-reviewer": {
      "description": "Reviews code for best practices and potential issues",
      "model": "anthropic/claude-sonnet-4-5",
      "prompt": "You are a code reviewer. Focus on security, performance, and maintainability.",
      "tools": {
        // Disable file modification tools for review-only agent
        "write": false,
        "edit": false,
      },
    },
  },
}
```

你也可以使用 `~/.config/opencode/agents/` 或 `.opencode/agents/` 中的 markdown 文件来定义 agents。[在此了解更多](/docs/agents)。

### Default agent

You can set the default agent using the `default_agent` option. This determines which agent is used when none is explicitly specified.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan"
}
```

The default agent must be a primary agent (not a subagent). This can be a built-in agent like `"build"` or `"plan"`, or a [custom agent](/docs/agents) you've defined. If the specified agent doesn't exist or is a subagent, OpenCode will fall back to `"build"` with a warning.

This setting applies across all interfaces: TUI, CLI (`opencode run`), desktop app, and GitHub Action.

### Default agent（默认智能体）

你可以使用 `default_agent` 选项设置默认 agent。这决定了当未明确指定时使用哪个 agent。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "default_agent": "plan"
}
```

默认 agent 必须是一个 primary agent（主智能体），而不能是 subagent（子智能体/被其他智能体调用的从属智能体）。这可以是像 `"build"` 或 `"plan"` 这样的内置 agent，也可以是你定义的[自定义 agent](/docs/agents)。如果指定的 agent 不存在或是 subagent，OpenCode 会回退到 `"build"` 并给出警告。

此设置适用于所有接口：TUI、CLI（`opencode run`）、桌面应用和 GitHub Action。

### Sharing

You can configure the [share](/docs/share) feature through the `share` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "share": "manual"
}
```

This takes:

- `"manual"` - Allow manual sharing via commands (default)
- `"auto"` - Automatically share new conversations
- `"disabled"` - Disable sharing entirely

By default, sharing is set to manual mode where you need to explicitly share conversations using the `/share` command.

### Sharing（分享）

你可以通过 `share` 选项配置[分享](/docs/share)功能。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "share": "manual"
}
```

该选项取值：

- `"manual"` - 允许通过命令手动分享（默认）
- `"auto"` - 自动分享新的对话
- `"disabled"` - 完全禁用分享

默认情况下，分享设置为手动模式，你需要使用 `/share` 命令显式地分享对话。

### Commands

You can configure custom commands for repetitive tasks through the `command` option.

opencode.jsonc

```
{
  "$schema": "https://opencode.ai/config.json",
  "command": {
    "test": {
      "template": "Run the full test suite with coverage report and show any failures.\nFocus on the failing tests and suggest fixes.",
      "description": "Run tests with coverage",
      "agent": "build",
      "model": "anthropic/claude-haiku-4-5",
    },
    "component": {
      "template": "Create a new React component named $ARGUMENTS with TypeScript support.\nInclude proper typing and basic structure.",
      "description": "Create a new component",
    },
  },
}
```

You can also define commands using markdown files in `~/.config/opencode/commands/` or `.opencode/commands/`. [Learn more here](/docs/commands).

### Commands（命令）

你可以通过 `command` 选项为重复性任务配置自定义 commands（命令）。

opencode.jsonc

```
{
  "$schema": "https://opencode.ai/config.json",
  "command": {
    "test": {
      "template": "Run the full test suite with coverage report and show any failures.\nFocus on the failing tests and suggest fixes.",
      "description": "Run tests with coverage",
      "agent": "build",
      "model": "anthropic/claude-haiku-4-5",
    },
    "component": {
      "template": "Create a new React component named $ARGUMENTS with TypeScript support.\nInclude proper typing and basic structure.",
      "description": "Create a new component",
    },
  },
}
```

你也可以使用 `~/.config/opencode/commands/` 或 `.opencode/commands/` 中的 markdown 文件来定义 commands。[在此了解更多](/docs/commands)。

### Keybinds

Customize TUI keyboard shortcuts in `tui.json` with `keybinds`.

tui.json

```
{
  "$schema": "https://opencode.ai/tui.json",
  "keybinds": {
    "command_list": "ctrl+p"
  }
}
```

`keybinds` is merged with built-in defaults, so you only need to configure the shortcuts you want to change.

[Learn more here](/docs/keybinds).

### Keybinds（快捷键）

在 `tui.json` 中使用 `keybinds` 自定义 TUI 键盘快捷键。

tui.json

```
{
  "$schema": "https://opencode.ai/tui.json",
  "keybinds": {
    "command_list": "ctrl+p"
  }
}
```

`keybinds` 会与内置默认值合并，因此你只需要配置想要更改的快捷键。

[在此了解更多](/docs/keybinds)。

### Snapshot

OpenCode uses snapshots to track file changes during agent operations, enabling you to undo and revert changes within a session. Snapshots are enabled by default.

For large repositories or projects with many submodules, the snapshot system can cause slow indexing and significant disk usage as it tracks all changes using an internal git repository. You can disable snapshots using the `snapshot` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "snapshot": false
}
```

Note that disabling snapshots means changes made by the agent cannot be rolled back through the UI.

### Snapshot（快照）

OpenCode 使用 snapshots（快照）来跟踪 agent 操作期间的文件变更，使你能够在会话中撤销和回滚变更。Snapshots 默认启用。

对于大型仓库或包含许多子模块的项目，snapshot 系统可能会导致索引缓慢和显著的磁盘占用，因为它使用一个内部 git 仓库来跟踪所有变更。你可以使用 `snapshot` 选项禁用 snapshots。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "snapshot": false
}
```

请注意，禁用 snapshots 意味着 agent 所做的变更无法通过 UI 回滚。

### Autoupdate

OpenCode will automatically download any new updates when it starts up. You can disable this with the `autoupdate` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "autoupdate": false
}
```

If you don't want updates but want to be notified when a new version is available, set `autoupdate` to `"notify"`. Notice that this only works if it was not installed using a package manager such as Homebrew.

### Autoupdate（自动更新）

OpenCode 会在启动时自动下载任何新的更新。你可以使用 `autoupdate` 选项禁用此功能。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "autoupdate": false
}
```

如果你不想自动更新但希望在有新版本可用时收到通知，可将 `autoupdate` 设置为 `"notify"`。请注意，这仅在 OpenCode 不是通过 Homebrew 等包管理器安装时才有效。

### Formatters

You can enable and configure code formatters through the `formatter` option. Omit it to keep formatters disabled.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": true
}
```

Use an object to keep built-ins enabled while configuring overrides or custom formatters.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": {
    "prettier": {
      "disabled": true
    },
    "custom-prettier": {
      "command": ["npx", "prettier", "--write", "$FILE"],
      "environment": {
        "NODE_ENV": "development"
      },
      "extensions": [".js", ".ts", ".jsx", ".tsx"]
    }
  }
}
```

[Learn more about formatters here](/docs/formatters).

### Formatters（代码格式化工具）

你可以通过 `formatter` 选项启用和配置代码 formatters（格式化工具）。省略此项则保持 formatters 禁用。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": true
}
```

使用对象形式可以在保持内置 formatters 启用的同时配置覆盖项或自定义 formatters。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "formatter": {
    "prettier": {
      "disabled": true
    },
    "custom-prettier": {
      "command": ["npx", "prettier", "--write", "$FILE"],
      "environment": {
        "NODE_ENV": "development"
      },
      "extensions": [".js", ".ts", ".jsx", ".tsx"]
    }
  }
}
```

[在此了解更多关于 formatters 的信息](/docs/formatters)。

### LSP Servers

You can enable and configure LSP servers through the `lsp` option. Omit it to keep LSP disabled.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": true
}
```

Use an object to keep built-ins enabled while configuring overrides or custom LSP servers.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": {
    "typescript": {
      "disabled": true
    }
  }
}
```

[Learn more about LSP servers here](/docs/lsp).

### LSP Servers（语言服务器）

你可以通过 `lsp` 选项启用和配置 LSP servers（语言服务器协议服务器）。省略此项则保持 LSP 禁用。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": true
}
```

使用对象形式可以在保持内置 LSP servers 启用的同时配置覆盖项或自定义 LSP servers。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "lsp": {
    "typescript": {
      "disabled": true
    }
  }
}
```

[在此了解更多关于 LSP servers 的信息](/docs/lsp)。

### Permissions

By default, opencode **allows all operations** without requiring explicit approval. You can change this using the `permission` option.

For example, to ensure that the `edit` and `bash` tools require user approval:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "ask",
    "bash": "ask"
  }
}
```

[Learn more about permissions here](/docs/permissions).

### Permissions（权限）

默认情况下，OpenCode **允许所有操作**而无需明确批准。你可以使用 `permission` 选项更改此行为。

例如，要确保 `edit` 和 `bash` 工具需要用户批准：

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "permission": {
    "edit": "ask",
    "bash": "ask"
  }
}
```

[在此了解更多关于 permissions 的信息](/docs/permissions)。

### Compaction

You can control context compaction behavior through the `compaction` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "compaction": {
    "auto": true,
    "prune": false,
    "reserved": 10000
  }
}
```

- `auto` - Automatically compact the session when context is full (default: `true`).
- `prune` - Remove old tool outputs to save tokens (default: `false`). Set to `true` to enable pruning.
- `reserved` - Token buffer for compaction. Leaves enough window to avoid overflow during compaction.

### Compaction（上下文压缩）

你可以通过 `compaction` 选项控制上下文压缩行为。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "compaction": {
    "auto": true,
    "prune": false,
    "reserved": 10000
  }
}
```

- `auto` - 当上下文已满时自动压缩会话（默认：`true`）。
- `prune` - 移除旧的工具输出以节省 tokens（默认：`false`）。设置为 `true` 可启用修剪。
- `reserved` - 用于压缩的 token 缓冲区。保留足够的窗口以避免压缩过程中溢出。

### Watcher

You can configure file watcher ignore patterns through the `watcher` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "watcher": {
    "ignore": ["node_modules/**", "dist/**", ".git/**"]
  }
}
```

Patterns follow glob syntax. Use this to exclude noisy directories from file watching.

### Watcher（文件监视器）

你可以通过 `watcher` 选项配置文件监视器的忽略模式。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "watcher": {
    "ignore": ["node_modules/**", "dist/**", ".git/**"]
  }
}
```

模式遵循 glob 语法。使用此项可将嘈杂的目录从文件监视中排除。

### MCP servers

You can configure MCP servers you want to use through the `mcp` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {}
}
```

[Learn more here](/docs/mcp-servers).

### MCP servers（MCP 服务器）

你可以通过 `mcp` 选项配置你想要使用的 MCP servers（Model Context Protocol 服务器）。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "mcp": {}
}
```

[在此了解更多](/docs/mcp-servers)。

### Plugins

[Plugins](/docs/plugins) extend OpenCode with custom tools, hooks, and integrations.

Place plugin files in `.opencode/plugins/` or `~/.config/opencode/plugins/`. You can also load plugins from npm through the `plugin` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": ["opencode-helicone-session", "@my-org/custom-plugin"]
}
```

[Learn more here](/docs/plugins).

### Plugins（插件）

[Plugins](/docs/plugins)（插件）通过自定义 tools、hooks（钩子/在特定事件发生时自动触发的回调机制）和集成来扩展 OpenCode。

将 plugin 文件放在 `.opencode/plugins/` 或 `~/.config/opencode/plugins/` 中。你也可以通过 `plugin` 选项从 npm 加载 plugins。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "plugin": ["opencode-helicone-session", "@my-org/custom-plugin"]
}
```

[在此了解更多](/docs/plugins)。

### Instructions

You can configure the instructions for the model you're using through the `instructions` option.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md", ".cursor/rules/*.md"]
}
```

This takes an array of paths and glob patterns to instruction files. [Learn more about rules here](/docs/rules).

### Instructions（指令）

你可以通过 `instructions` 选项为你所使用的模型配置 instructions（指令/给模型的提示内容）。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["CONTRIBUTING.md", "docs/guidelines.md", ".cursor/rules/*.md"]
}
```

此项接受一个路径和 glob 模式数组，指向指令文件。[在此了解更多关于 rules（规则）的信息](/docs/rules)。

### Disabled providers

You can disable providers that are loaded automatically through the `disabled_providers` option. This is useful when you want to prevent certain providers from being loaded even if their credentials are available.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "disabled_providers": ["openai", "gemini"]
}
```

Note

The `disabled_providers` takes priority over `enabled_providers`.

The `disabled_providers` option accepts an array of provider IDs. When a provider is disabled:

- It won't be loaded even if environment variables are set.
- It won't be loaded even if API keys are configured through the `/connect` command.
- The provider's models won't appear in the model selection list.

### Disabled providers（禁用的提供者）

你可以通过 `disabled_providers` 选项禁用自动加载的 providers。当你想要阻止某些 providers 被加载时，即使它们的凭据可用，这也很有用。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "disabled_providers": ["openai", "gemini"]
}
```

注意

`disabled_providers` 的优先级高于 `enabled_providers`。

`disabled_providers` 选项接受一个 provider ID 数组。当一个 provider 被禁用时：

- 即使设置了环境变量，它也不会被加载。
- 即使通过 `/connect` 命令配置了 API 密钥，它也不会被加载。
- 该 provider 的 models 不会出现在模型选择列表中。

### Enabled providers

You can specify an allowlist of providers through the `enabled_providers` option. When set, only the specified providers will be enabled and all others will be ignored.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "enabled_providers": ["anthropic", "openai"]
}
```

This is useful when you want to restrict OpenCode to only use specific providers rather than disabling them one by one.

Note

The `disabled_providers` takes priority over `enabled_providers`.

If a provider appears in both `enabled_providers` and `disabled_providers`, the `disabled_providers` takes priority for backwards compatibility.

### Enabled providers（启用的提供者）

你可以通过 `enabled_providers` 选项指定一个 providers 的允许列表。设置后，只有指定的 providers 会被启用，其他所有 providers 都会被忽略。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "enabled_providers": ["anthropic", "openai"]
}
```

当你想要将 OpenCode 限制为仅使用特定的 providers，而不是逐一禁用它们时，这很有用。

注意

`disabled_providers` 的优先级高于 `enabled_providers`。

如果一个 provider 同时出现在 `enabled_providers` 和 `disabled_providers` 中，出于向后兼容，`disabled_providers` 优先。

### Experimental

The `experimental` key contains options that are under active development.

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "experimental": {}
}
```

Caution

Experimental options are not stable. They may change or be removed without notice.

### Experimental（实验性功能）

`experimental` 键包含正在积极开发中的选项。

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "experimental": {}
}
```

注意

实验性选项不稳定。它们可能会更改或被移除，恕不另行通知。

### Variables

You can use variable substitution in your config files to reference environment variables and file contents.

### Variables（变量）

你可以在配置文件中使用变量替换来引用环境变量和文件内容。

### Env vars

Use `{env:VARIABLE_NAME}` to substitute environment variables:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "model": "{env:OPENCODE_MODEL}",
  "provider": {
    "anthropic": {
      "models": {},
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    }
  }
}
```

If the environment variable is not set, it will be replaced with an empty string.

### Env vars（环境变量）

使用 `{env:VARIABLE_NAME}` 来替换环境变量：

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "model": "{env:OPENCODE_MODEL}",
  "provider": {
    "anthropic": {
      "models": {},
      "options": {
        "apiKey": "{env:ANTHROPIC_API_KEY}"
      }
    }
  }
}
```

如果环境变量未设置，它将被替换为空字符串。

### Files

Use `{file:path/to/file}` to substitute the contents of a file:

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["./custom-instructions.md"],
  "provider": {
    "openai": {
      "options": {
        "apiKey": "{file:~/.secrets/openai-key}"
      }
    }
  }
}
```

File paths can be:

- Relative to the config file directory
- Or absolute paths starting with `/` or `~`

These are useful for:

- Keeping sensitive data like API keys in separate files.
- Including large instruction files without cluttering your config.
- Sharing common configuration snippets across multiple config files.

### Files（文件）

使用 `{file:path/to/file}` 来替换文件的内容：

opencode.json

```
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["./custom-instructions.md"],
  "provider": {
    "openai": {
      "options": {
        "apiKey": "{file:~/.secrets/openai-key}"
      }
    }
  }
}
```

文件路径可以是：

- 相对于配置文件目录的路径
- 或以 `/` 或 `~` 开头的绝对路径

这些功能适用于：

- 将 API 密钥等敏感数据保存在单独的文件中。
- 包含大型指令文件而不会使你的配置变得杂乱。
- 在多个配置文件之间共享通用的配置片段。


---

## Google - Code Review（代码审查）

原文链接：https://google.github.io/eng-practices/review/reviewer/

### Introduction

A code review is a process where someone other than the author(s) of a piece of code examines that code.

代码审查（code review，指由非作者人员检查代码的过程）是这样一种流程：由代码作者以外的人来检查某段代码。

At Google, we use code review to maintain the quality of our code and products.

在 Google，我们通过代码审查来保持代码和产品的质量。

This documentation is the canonical description of Google's code review processes and policies.

本文档是对 Google 代码审查流程与策略的权威性描述。

This page is an overview of our code review process. There are two other large documents that are a part of this guide:

本页是对我们代码审查流程的概览。本指南还包含另外两篇较长的文档：

-   **[How To Do A Code Review](/eng-practices/review/reviewer/)**: A detailed guide for code reviewers.
-   **[The CL Author's Guide](/eng-practices/review/developer/)**: A detailed guide for developers whose CLs are going through review.

-   **[如何进行代码审查（How To Do A Code Review）](/eng-practices/review/reviewer/)**：面向代码审查者的详细指南。
-   **[CL（changelist，变更列表）作者指南（The CL Author's Guide）](/eng-practices/review/developer/)**：面向其 CL 正在接受审查的开发者的详细指南。

### What Do Code Reviewers Look For?

### 代码审查者关注什么？

Code reviews should look at:

代码审查应当关注以下方面：

-   **Design**: Is the code well-designed and appropriate for your system?
-   **Functionality**: Does the code behave as the author likely intended? Is the way the code behaves good for its users?
-   **Complexity**: Could the code be made simpler? Would another developer be able to easily understand and use this code when they come across it in the future?
-   **Tests**: Does the code have correct and well-designed automated tests?
-   **Naming**: Did the developer choose clear names for variables, classes, methods, etc.?
-   **Comments**: Are the comments clear and useful?
-   **Style**: Does the code follow our [style guides](http://google.github.io/styleguide/)?
-   **Documentation**: Did the developer also update relevant documentation?

-   **设计（Design）**：代码设计是否良好，是否适合你的系统？
-   **功能（Functionality）**：代码行为是否符合作者预期的意图？代码的行为方式对它的用户是否友好？
-   **复杂度（Complexity）**：代码能否做得更简洁？未来其他开发者接触到这段代码时，能否轻松理解并使用它？
-   **测试（Tests）**：代码是否有正确且设计良好的自动化测试？
-   **命名（Naming）**：开发者是否为变量、类、方法等选择了清晰的名称？
-   **注释（Comments）**：注释是否清晰且有用？
-   **风格（Style）**：代码是否遵循我们的[风格指南（style guide）](http://google.github.io/styleguide/)？
-   **文档（Documentation）**：开发者是否也更新了相关文档？

See **[How To Do A Code Review](/eng-practices/review/reviewer/)** for more information.

更多信息请参阅 **[如何进行代码审查（How To Do A Code Review）](/eng-practices/review/reviewer/)**。

### Picking the Best Reviewers

### 选择最佳审查者

In general, you want to find the *best* reviewers you can who are capable of responding to your review within a reasonable period of time.

一般而言，你希望找到尽可能*最好的*审查者（reviewer），并且他们能在合理时间内对你的审查作出回应。

The best reviewer is the person who will be able to give you the most thorough and correct review for the piece of code you are writing. This usually means the owner(s) of the code, who may or may not be the people in the OWNERS file. Sometimes this means asking different people to review different parts of the CL.

最佳审查者是能够对你所写代码给出最彻底、最正确审查的人。这通常意味着代码的所有者，他们可能、也可能不是 OWNERS 文件（代码所有权文件）中列出的人。有时这意味着请不同的人来审查 CL 的不同部分。

If you find an ideal reviewer but they are not available, you should at least CC them on your change.

如果你找到了理想的审查者但他无法参与，至少应在你的变更中抄送（CC）他。

### In-Person Reviews (and Pair Programming)

### 当面审查（以及结对编程）

If you pair-programmed a piece of code with somebody who was qualified to do a good code review on it, then that code is considered reviewed.

如果你与某人结对编程（pair programming，两人共用一个键盘协作编写代码）编写了一段代码，而此人有资格对该代码做出良好的审查，那么这段代码即视为已通过审查。

You can also do in-person code reviews where the reviewer asks questions and the developer of the change speaks only when spoken to.

你也可以进行当面代码审查：由审查者提问，变更的开发者仅在被问到时才发言。

### See Also

### 另请参阅

-   [How To Do A Code Review](/eng-practices/review/reviewer/): A detailed guide for code reviewers.
-   [The CL Author's Guide](/eng-practices/review/developer/): A detailed guide for developers whose CLs are going through review.

-   [如何进行代码审查（How To Do A Code Review）](/eng-practices/review/reviewer/)：面向代码审查者的详细指南。
-   [CL 作者指南（The CL Author's Guide）](/eng-practices/review/developer/)：面向其 CL 正在接受审查的开发者的详细指南。

This site is open source. [Improve this page](https://github.com/google/eng-practices/edit/master/review/index.md).

本网站为开源项目。可在 GitHub 上[改进本页](https://github.com/google/eng-practices/edit/master/review/index.md)。


---

## Google - Small CLs（小变更）

原文链接：https://google.github.io/eng-practices/review/developer/small-cls.html

### Small CLs

### 小型 CL

## Why Write Small CLs?

Small, simple CLs are:

-   **Reviewed more quickly.** It's easier for a reviewer to find five minutes several times to review small CLs than to set aside a 30 minute block to review one large CL.
-   **Reviewed more thoroughly.** With large changes, reviewers and authors tend to get frustrated by large volumes of detailed commentary shifting back and forth—sometimes to the point where important points get missed or dropped.
-   **Less likely to introduce bugs.** Since you're making fewer changes, it's easier for you and your reviewer to reason effectively about the impact of the CL and see if a bug has been introduced.
-   **Less wasted work if they are rejected.** If you write a huge CL and then your reviewer says that the overall direction is wrong, you've wasted a lot of work.
-   **Easier to merge.** Working on a large CL takes a long time, so you will have lots of conflicts when you merge, and you will have to merge frequently.
-   **Easier to design well.** It's a lot easier to polish the design and code health of a small change than it is to refine all the details of a large change.
-   **Less blocking on reviews.** Sending self-contained portions of your overall change allows you to continue coding while you wait for your current CL in review.
-   **Simpler to roll back.** A large CL will more likely touch files that get updated between the initial CL submission and a rollback CL, complicating the rollback (the intermediate CLs will probably need to be rolled back too).

## 为什么要写小型 CL？

小型、简单的 CL（changelist，变更清单）具有以下优点：

-   **审查 (code review，代码审查) 更快。** 让审查者 (reviewer) 抽出几次五分钟来审查小型 CL，比让他专门留出 30 分钟来审查一个大型 CL 要容易得多。
-   **审查更彻底。** 对于大型变更，审查者和作者 (author) 往往会被大量来回往复的详细评论弄得疲惫不堪——有时甚至会导致重要问题被遗漏或丢失。
-   **更不容易引入 bug。** 由于改动更少，你和审查者更容易有效地推断出该 CL 的影响，并看清是否引入了 bug。
-   **被拒绝时浪费的工作更少。** 如果你写了一个庞大的 CL，而审查者说整体方向错了，那你就浪费了大量工作。
-   **更容易合并 (merge)。** 做一个大型 CL 需要很长时间，因此在合并时会有很多冲突，而且不得不频繁合并。
-   **更容易做好设计。** 打磨一个小改动的设计和代码健康度，要比精雕细琢一个大改动的所有细节容易得多。
-   **审查时更少阻塞。** 把整体变更中自成体系的部分单独发出去，可以让你在等待当前 CL 审查时继续编码。
-   **更容易回滚 (roll back)。** 一个大型 CL 更可能触及那些在初始 CL 提交和回滚 CL 之间被更新的文件，从而使回滚变得复杂（中间的那些 CL 可能也得一并回滚）。

Note that **reviewers have discretion to reject your change outright for the sole reason of it being too large.** Usually they will thank you for your contribution but request that you somehow make it into a series of smaller changes. It can be a lot of work to split up a change after you've already written it, or require lots of time arguing about why the reviewer should accept your large change. It's easier to just write small CLs in the first place.

需要注意的是，**审查者有权仅以"改动过大"为由直接拒绝你的变更。** 通常他们会感谢你的贡献，但会要求你以某种方式将其拆成一系列更小的改动。在你已经写完之后再来拆分一个改动会很费功夫，或者需要花费大量时间去争论为什么审查者应当接受你的大型改动。还不如一开始就写小型 CL。

## What is Small?

In general, the right size for a CL is **one self-contained change**. This means that:

-   The CL makes a minimal change that addresses **just one thing**. This is usually just one part of a feature, rather than a whole feature at once. In general it's better to err on the side of writing CLs that are too small vs. CLs that are too large. Work with your reviewer to find out what an acceptable size is.
-   The CL should [include related test code](#test_code).
-   Everything the reviewer needs to understand about the CL (except future development) is in the CL, the CL's description, the existing codebase, or a CL they've already reviewed.
-   The system will continue to work well for its users and for the developers after the CL is checked in.
-   The CL is not so small that its implications are difficult to understand. If you add a new API, you should include a usage of the API in the same CL so that reviewers can better understand how the API will be used. This also prevents checking in unused APIs.

## 多小才算小？

一般来说，CL 的合适大小是**一个自包含的变更**。这意味着：

-   该 CL 做出的是只针对**一件事**的最小改动。这通常只是一个特性的一部分，而不是一次把整个特性全做完。总体而言，宁可把 CL 写得过小，也不要写得过大。和你的审查者一起确认可接受的大小。
-   该 CL 应当[包含相关的测试代码](#test_code)。
-   审查者理解该 CL 所需的一切（除未来的开发外）都在该 CL、CL 的描述、现有代码库，或者他们已经审查过的某个 CL 中。
-   在该 CL 提交 (check in) 之后，系统对用户和开发者仍能继续良好运行。
-   该 CL 不能小到其含义难以理解。如果你新增了一个 API，就应该在同一个 CL 中包含对该 API 的使用，这样审查者才能更好地理解该 API 将如何被使用。这也避免了提交未被使用的 API。

There are no hard and fast rules about how large is "too large." 100 lines is usually a reasonable size for a CL, and 1000 lines is usually too large, but it's up to the judgment of your reviewer. The number of files that a change is spread across also affects its "size." A 200-line change in one file might be okay, but spread across 50 files it would usually be too large.

关于"过大"到底有多大，并没有硬性规定。100 行通常是一个合理的 CL 大小，1000 行通常就太大了，但这要由审查者来判断。一个改动所涉及的文件数量也会影响它的"大小"。在一个文件里改 200 行可能没问题，但分散到 50 个文件里通常就太大了。

Keep in mind that although you have been intimately involved with your code from the moment you started to write it, the reviewer often has no context. What seems like an acceptably-sized CL to you might be overwhelming to your reviewer. When in doubt, write CLs that are smaller than you think you need to write. Reviewers rarely complain about getting CLs that are too small.

请记住，尽管你从开始写代码的那一刻起就与自己的代码紧密相伴，但审查者往往毫无上下文。在你看来大小可接受的 CL，对审查者来说可能已经让人应接不暇。拿不准时，就把 CL 写得比你认为需要的更小一些。审查者极少会抱怨收到的 CL 太小。

## When are Large CLs Okay?

There are a few situations in which large changes aren't as bad:

-   You can usually count deletion of an entire file as being just one line of change, because it doesn't take the reviewer very long to review.
-   Sometimes a large CL has been generated by an automatic refactoring tool that you trust completely, and the reviewer's job is just to verify and say that they really do want the change. These CLs can be larger, although some of the caveats from above (such as merging and testing) still apply.

## 什么时候大型 CL 是可以接受的？

有少数几种情况，大的改动没那么糟糕：

-   通常你可以把删除一整个文件算作只改了一行，因为审查者花不了多少时间就能审查完。
-   有时一个大型 CL 是由你完全信任的自动重构 (refactoring，指在不改变外部行为的前提下调整代码结构) 工具生成的，审查者的工作只是核实并确认他们确实想要这个改动。这类 CL 可以更大一些，不过上文提到的一些注意事项（如合并和测试）依然适用。

## Writing Small CLs Efficiently

If you write a small CL and then you wait for your reviewer to approve it before you write your next CL, then you're going to waste a lot of time. So you want to find some way to work that won't block you while you're waiting for review. This could involve having multiple projects to work on simultaneously, finding reviewers who agree to be immediately available, doing in-person reviews, pair programming, or splitting your CLs in a way that allows you to continue working immediately.

## 高效地编写小型 CL

如果你写完一个小 CL 后，就一直等着审查者批准了再写下一个 CL，那你就会浪费大量时间。所以你需要找到某种不会在等待审查时阻塞自己的工作方式。这可以包括：同时进行多个项目、找同意随叫随到的审查者、做当面审查、结对编程，或者用一种能让你立即继续工作的方式来拆分 CL。

## Splitting CLs

When starting work that will have multiple CLs with potential dependencies among each other, it's often useful to think about how to split and organize those CLs at a high level before diving into coding.

Besides making things easier for you as an author to manage and organize your CLs, it also makes things easier for your code reviewers, which in turn makes your code reviews more efficient.

Here are some strategies for splitting work into different CLs.

## 拆分 CL

当你开始一项会产生多个 CL、且它们之间可能存在相互依赖的工作时，在投入编码之前，先从较高层次思考如何拆分和组织这些 CL，往往很有帮助。

除了让你作为作者更便于管理和组织自己的 CL 之外，这也会让代码审查者的工作更轻松，进而使你的代码审查更高效。

下面是一些把工作拆分成不同 CL 的策略。

### Stacking Multiple Changes on Top of Each Other

One way to split up a CL without blocking yourself is to write one small CL, send it off for review, and then immediately start writing another CL *based* on the first CL. Most version control systems allow you to do this somehow.

### 将多个改动层层堆叠

一种既能拆分 CL 又不阻塞自己的办法是：写一个小 CL，发出去审查，然后立即基于第一个 CL 开始写另一个 CL。大多数版本控制系统 (version control system) 都能以某种方式做到这一点。

### Splitting by Files

Another way to split up a CL is by groupings of files that will require different reviewers but are otherwise self-contained changes.

For example: you send off one CL for modifications to a protocol buffer and another CL for changes to the code that uses that proto. You have to submit the proto CL before the code CL, but they can both be reviewed simultaneously. If you do this, you might want to inform both sets of reviewers about the other CL that you wrote, so that they have context for your changes.

Another example: you send one CL for a code change and another for the configuration or experiment that uses that code; this is easier to roll back too, if necessary, as configuration/experiment files are sometimes pushed to production faster than code changes.

### 按文件拆分

另一种拆分 CL 的方式是按文件分组，这些分组需要不同的审查者，但各自都是自包含的改动。

例如：你发一个 CL 修改 protocol buffer（协议缓冲区，Google 的数据交换格式，常简称 proto），再发另一个 CL 修改使用该 proto 的代码。你必须先提交 proto 的 CL，再提交代码的 CL，但两者可以同时被审查。如果你这样做，可能需要告知两组审查者你还写了另一个 CL，以便他们了解你改动的上下文。

另一个例子：你发一个 CL 做代码改动，再发另一个 CL 做使用该代码的配置或实验；这在需要时也更容易回滚，因为配置/实验文件有时比代码改动更快地被推送到生产环境。

### Splitting Horizontally

Consider creating shared code or stubs that help isolate changes between layers of the tech stack. This not only helps expedite development but also encourages abstraction between layers.

For example: You created a calculator app with client, API, service, and data model layers. A shared proto signature can abstract the service and data model layers from each other. Similarly, an API stub can split the implementation of client code from service code and enable them to move forward independently. Similar ideas can also be applied to more granular function or class level abstractions.

### 水平拆分

可以考虑创建共享代码或桩 (stub，指接口的占位实现)，用以隔离技术栈各层之间的改动。这不仅能加快开发速度，还能促进各层之间的抽象。

例如：你创建了一个计算器应用，包含客户端、API、服务和数据模型等层。一个共享的 proto 签名可以把服务层与数据模型层彼此抽象隔离。类似地，一个 API 桩可以把客户端代码的实现与服务代码的实现拆开，使两者能独立推进。类似的想法也可应用于更细粒度的函数或类级别的抽象。

### Splitting Vertically

Orthogonal to the layered, horizontal approach, you can instead break down your code into smaller, full-stack, vertical features. Each of these features can be independent parallel implementation tracks. This enables some tracks to move forward while other tracks are awaiting review or feedback.

Back to our calculator example from [Splitting Horizontally](#splitting-horizontally). You now want to support new operators, like multiplication and division. You could split this up by implementing multiplication and division as separate verticals or sub-features, even though they may have some overlap such as shared button styling or shared validation logic.

### 垂直拆分

与分层、水平的做法正交，你可以转而把代码拆解成更小的、贯穿全栈的垂直特性。其中每个特性都可以是独立的并行实现轨道。这样某些轨道就能继续推进，而另一些轨道还在等待审查或反馈。

回到[水平拆分](#splitting-horizontally)中的计算器例子。现在你想支持新的运算符，比如乘法和除法。你可以把乘法和除法作为各自独立的垂直特性或子特性来拆分实现，即便它们可能会有一些重叠，比如共享的按钮样式或共享的校验逻辑。

### Splitting Horizontally & Vertically

To take this a step further, you could combine these approaches and chart out an implementation plan like this, where each cell is its own standalone CL. Starting from the model (at the bottom) and working up to the client:

| Layer | Feature: Multiplication | Feature: Division |
| --- | --- | --- |
| Client | Add button | Add button |
| API | Add endpoint | Add endpoint |
| Service | Implement transformations | Share transformation logic with multiplication |
| Model | Add proto definition | Add proto definition |

### 同时进行水平与垂直拆分

更进一步，你可以把这些方法结合起来，规划出这样的实现方案，其中每个单元格都是它自己独立的 CL。从模型（在最底层）开始，一直向上做到客户端：

| 层 | 特性：乘法 | 特性：除法 |
| --- | --- | --- |
| 客户端 | 添加按钮 | 添加按钮 |
| API | 添加 endpoint | 添加 endpoint |
| 服务 | 实现转换 | 与乘法共享转换逻辑 |
| 模型 | 添加 proto 定义 | 添加 proto 定义 |

## Separate Out Refactorings

It's usually best to do refactorings in a separate CL from feature changes or bug fixes. For example, moving and renaming a class should be in a different CL from fixing a bug in that class. It is much easier for reviewers to understand the changes introduced by each CL when they are separate.

Small cleanups such as fixing a local variable name can be included inside of a feature change or bug fix CL, though. It's up to the judgment of developers and reviewers to decide when a refactoring is so large that it will make the review more difficult if included in your current CL.

## 把重构单独拆出来

通常最好把重构放在一个独立的 CL 中，与特性改动或 bug 修复分开。例如，移动并重命名一个类，应该和修复该类中的 bug 放在不同的 CL 里。当它们分开时，审查者更容易理解每个 CL 所引入的改动。

不过，像修正一个局部变量名这类小清理，可以包含在特性改动或 bug 修复的 CL 中。至于一个重构何时大到如果放进当前 CL 会让审查变得更困难，就要由开发者和审查者来判断了。

## Keep related test code in the same CL

CLs should include related test code. Remember that [smallness](#what-is-small) here refers the conceptual idea that the CL should be focused and is not a simplistic function on line count.

Tests are expected for all Google changes.

A CL that adds or changes logic should be accompanied by new or updated tests for the new behavior. Pure refactoring CLs (that aren't intended to change behavior) should also be covered by tests; ideally, these tests already exist, but if they don't, you should add them.

*Independent* test modifications can go into separate CLs first, similar to the [refactorings guidelines](#refactoring). That includes:

-   Validating pre-existing, submitted code with new tests.
    -   Ensures that important logic is covered by tests.
    -   Increases confidence in subsequent refactorings on affected code. For example, if you want to refactor code that isn't already covered by tests, submitting test CLs *before* submitting refactoring CLs can validate that the tested behavior is unchanged before and after the refactoring.
-   Refactoring the test code (e.g. introduce helper functions).
-   Introducing larger test framework code (e.g. an integration test).

## 把相关的测试代码放在同一个 CL 中

CL 应当包含相关的测试代码。请记住，这里的[小](#what-is-small)是一个概念上的理念，指的是 CL 应当聚焦，而不是简单地以行数来衡量。

Google 的所有改动都要求有测试。

新增或改动逻辑的 CL，应当附带为新行为编写的新测试或更新的测试。纯粹的重构 CL（不打算改变行为的）也应当被测试覆盖；理想情况下这些测试已经存在，但如果没有，你就应当补上。

*独立的*测试修改可以先放进单独的 CL，这与[重构的准则](#refactoring)类似。这包括：

-   用新测试验证已提交的既有代码。
    -   确保重要逻辑被测试覆盖。
    -   提升对受影响代码后续重构的信心。例如，如果你想重构尚未被测试覆盖的代码，那么在提交重构 CL *之前*先提交测试 CL，可以验证被测试的行为在重构前后保持不变。
-   重构测试代码（例如引入辅助函数）。
-   引入较大的测试框架代码（例如一个集成测试）。

## Don't Break the Build

If you have several CLs that depend on each other, you need to find a way to make sure the whole system keeps working after each CL is submitted. Otherwise you might break the build for all your fellow developers for a few minutes between your CL submissions (or even longer if something goes wrong unexpectedly with your later CL submissions).

## 不要破坏构建

如果你有多个相互依赖的 CL，就需要想办法确保每提交一个 CL 之后整个系统仍然能正常工作。否则，你可能在各次 CL 提交之间的几分钟内破坏构建 (build)，让所有同事都受影响（如果后续的 CL 提交出了意外，受影响的时间甚至更长）。

## Can't Make it Small Enough

Sometimes you will encounter situations where it seems like your CL *has* to be large. This is very rarely true. Authors who practice writing small CLs can almost always find a way to decompose functionality into a series of small changes.

Before writing a large CL, consider whether preceding it with a refactoring-only CL could pave the way for a cleaner implementation. Talk to your teammates and see if anybody has thoughts on how to implement the functionality in small CLs instead.

If all of these options fail (which should be extremely rare) then get consent from your reviewers in advance to review a large CL, so they are warned about what is coming. In this situation, expect to be going through the review process for a long time, be vigilant about not introducing bugs, and be extra diligent about writing tests.

## 实在无法拆到足够小

有时你会遇到似乎 CL *必须*很大的情况。这极少是真的。练习写小型 CL 的作者几乎总能找到办法，把功能拆解成一系列小改动。

在写大型 CL 之前，先考虑一下是否可以用一个仅做重构的 CL 来铺路，从而获得更干净的实现。和你的队友聊聊，看看有没有人对于如何用小型 CL 来实现这个功能有想法。

如果所有这些办法都行不通（这应当极其罕见），那就事先征得审查者同意来审查一个大型 CL，好让他们对即将到来的东西有所准备。在这种情况下，要做好经历漫长审查过程的心理准备，对不引入 bug 保持高度警惕，并且在编写测试上格外用心。

Next: [How to Handle Reviewer Comments](/eng-practices/review/developer/handling-comments.html)

下一篇：[如何处理审查者的评论](/eng-practices/review/developer/handling-comments.html)


---

## DORA - Accelerate State of DevOps 2025

原文链接：https://cloud.google.com/devops/state-of-devops

### Report Overview / 报告概述

In 2025, the central question for technology leaders is no longer if they should adopt AI, but how to realize its value. DORA's research includes more than 100 hours of qualitative data and survey responses from nearly 5,000 technology professionals from around the world. The research reveals a critical truth: AI's primary role in software development is that of an amplifier. It magnifies the strengths of high-performing organizations and the dysfunctions of struggling ones.

2025 年，技术领导者面临的核心问题已不再是"是否应该采用 AI"，而是"如何实现其价值"。DORA 的研究包含超过 100 小时的定性数据，以及来自全球近 5,000 名技术专业人士的问卷调查反馈。研究揭示了一个关键事实：AI 在软件开发中的主要角色是"放大器"(amplifier)。它既放大高绩效组织的优势，也放大 struggling（表现不佳）组织的功能障碍。

### Key Finding: AI as an Amplifier / 核心发现：AI 作为放大器

The State of AI-assisted Software Development report reveals AI's primary role is as an amplifier, magnifying an organization's existing strengths and weaknesses. The greatest returns on AI investment come not from the tools themselves, but from a strategic focus on the underlying organizational system.

《AI 辅助软件开发现状》报告揭示，AI 的主要角色是放大器，会放大组织既有的优势与弱点。AI 投资的最大回报并非来自工具本身，而是来自对底层组织体系的战略性关注。

### AI Adoption is Near-Universal / AI 采用已近乎普及

AI adoption is near-universal: 90% of survey respondents report using AI at work. More than 80% believe it has increased their productivity. This year's report reveals a significant finding: AI adoption among software development professionals has surged to 90%, marking a 14% increase from 76% in 2024.

AI 采用已近乎普及：90% 的受访者表示在工作中使用 AI。超过 80% 认为它提升了自身生产力。今年的报告揭示了一项重要发现：软件开发专业人士的 AI 采用率飙升至 90%，较 2024 年的 76% 增长了 14 个百分点。

### How Developers Use AI / 开发者如何使用 AI

The highest use of AI is for writing new code (71% of developers), and the most common interaction with AI is via chatbots (among all users, not all of whom are coders), followed by IDEs (integrated development environments). Use of agent mode, where AI makes changes autonomously, is less common, with 61% saying they never do this and only 17% doing so once a day or more often.

AI 的最高频用途是编写新代码（71% 的开发者），最常见的 AI 交互方式是 chatbot（聊天机器人）（针对所有用户，并非全部是编码人员），其次是 IDE（集成开发环境）。agent mode（智能体模式，即 AI 自主进行代码修改）的使用相对较少，61% 的人表示从未使用，仅 17% 的人每天使用一次或更多。

### Trust in AI-Generated Code / 对 AI 生成代码的信任度

Although 80% of those surveyed believe AI has increased productivity, there are drawbacks. 30% do not trust AI-generated code. While AI adoption increased significantly, respondents' trust in the technology didn't improve proportionately: 30% of respondents said they trust AI "a little" or "not at all," down from 39.2% last year, but this year 70% said they trusted AI-generated outputs "somewhat," "a lot," or "a great deal," versus 87.9% in 2024.

尽管 80% 的受访者认为 AI 提升了生产力，但仍存在弊端。30% 的人不信任 AI 生成的代码。虽然 AI 采用率显著上升，但受访者对该技术的信任度并未同步提升：30% 的受访者表示对 AI "只信任一点"或"完全不信任"，低于去年的 39.2%；但今年仅 70% 表示"在一定程度上"、"较多"或"非常"信任 AI 生成结果，而 2024 年这一比例为 87.9%。

### Impact on Software Delivery / 对软件交付的影响

The Google research group's survey measures software delivery performance in two main categories: speed and efficiency, or throughput, and quality and reliability of releases, termed instability. Last year, DORA's survey of 3,000 respondents found a decrease of 1.5% in software delivery throughput and a 7.2% decrease in delivery stability for every 25% increase in an organization's AI adoption.

Google 研究组的调查从两大维度衡量软件交付绩效：速度与效率（即 throughput/吞吐量），以及发布的质量与可靠性（即 instability/不稳定度）。去年的 DORA 调查（3,000 名受访者）发现，组织 AI 采用率每提高 25%，软件交付吞吐量下降 1.5%，交付稳定性下降 7.2%。

This year, among 5,000 survey respondents and more than 100 hours of research interviews, those outcomes were measured differently, but significantly differed from last year's results. Google DORA found the use of AI coding tools no longer correlate with software delivery throughput slowdowns, but still pose instability issues. AI increases delivery instability, and overall AI acts as an amplifier, increasing the strength of high-performing organizations but worsening the dysfunction of those that struggle.

今年的调查（5,000 名受访者、超过 100 小时研究访谈）采用了不同的衡量方式，但结果与去年显著不同。Google DORA 发现，AI 编码工具的使用不再与软件交付吞吐量放缓相关，但仍会带来稳定性问题。AI 会增加交付不稳定度，总体而言 AI 充当放大器——增强高绩效组织的实力，同时加剧 struggling 组织的功能障碍。

### The Shift in DORA's Focus / DORA 焦点的转变

The 2025 report represents a dramatic shift in focus towards AI for the DORA project. The 2024 report also featured AI, though its conclusions were mixed and measured. "AI does not appear to be a panacea," it reported at the time, and although there was evidence in favor of adopting AI, "there are plenty of potential roadblocks, growing pains, and ways in which AI might have deleterious effects." One such effect was reduced software delivery stability and throughput.

2025 年报告标志着 DORA 项目在 AI 方向上的重大焦点转变。2024 年报告虽也涉及 AI，但结论较为审慎、褒贬不一。当时报告指出"AI 似乎并非灵丹妙药"，尽管有证据支持采用 AI，但"存在大量潜在障碍、成长阵痛，以及 AI 可能产生有害影响的途径"。其中一项影响就是软件交付稳定性与吞吐量的下降。

In 2024 and in previous years, the DORA research assessed software delivery performance based on four keys: change lead time between code commit and deployment, deployment frequency, change fail rate, and failed deployment recovery time. In 2025, these are referenced only in a footnote, as one of many ways to measure software development. The DORA report is now called the "State of AI-assisted software development" where previously it was called the "Accelerate State of DevOps."

在 2024 年及之前，DORA 研究基于四个关键指标评估软件交付绩效：代码提交到部署的变更前置时间(change lead time)、部署频率(deployment frequency)、变更失败率(change fail rate)以及失败部署恢复时间。2025 年，这些指标仅在脚注中提及，作为衡量软件开发的众多方式之一。DORA 报告现更名为"AI 辅助软件开发现状"，而此前名为"Accelerate State of DevOps"。

### Introducing the DORA AI Capabilities Model / DORA AI 能力模型介绍

Artificial intelligence is rapidly transforming software development. But simply adopting AI tools isn't a guarantee of success. Across the industry, tech leaders and developers are asking the same critical questions: How do we move from just using AI to truly succeeding with it? How do we ensure our investment in AI delivers better, faster, and more reliable software? The DORA research team has developed the inaugural DORA AI Capabilities Model to provide data-backed guidance for organizations grappling with these questions.

人工智能正在快速变革软件开发。但仅仅采用 AI 工具并不能保证成功。整个行业的技术领导者和开发者都在追问同样的关键问题：如何从"仅仅使用 AI"迈向"真正用好 AI"？如何确保 AI 投资带来更优、更快、更可靠的软件？DORA 研究团队开发了首届 DORA AI 能力模型(AI Capabilities Model)，为正在应对这些问题的组织提供数据支撑的指导。

This is not just another report on AI adoption trends; it is a guide to the specific technical and cultural practices that amplify the benefits of AI. We developed the DORA AI Capabilities Model through a three-phase process. First, we identified and prioritized a wide-range of candidate capabilities based on 78 in-depth interviews, existing literature, and perspectives from leading subject-matter experts. Second, we developed and validated survey questions to ensure they were clear, reliable, and measured each capability accurately. Lastly, we evaluated the impact of a subset of these candidates using the rigorous methodology of designing and analyzing our annual survey—which reached almost 5,000 respondents.

这并非又一份关于 AI 采用趋势的报告，而是一份针对能放大 AI 收益的具体技术与文化实践的指南。我们通过三阶段流程开发了 DORA AI 能力模型。第一阶段，基于 78 次深度访谈、现有文献及顶尖领域专家的观点，识别并优先排序了广泛的候选能力。第二阶段，开发并验证了调查问卷，确保其清晰、可靠，能准确衡量每项能力。最后，运用年度调查（覆盖近 5,000 名受访者）的严谨设计与分析方法，评估了部分候选能力的影响。

### The Seven Foundational Capabilities / 七大基础能力

The analysis identified seven capabilities that substantially either amplify or unlock the benefits of AI. The value of AI is unlocked by the surrounding technical and cultural environment. These seven foundational capabilities—spanning both technical and cultural domains—are proven to amplify the positive impact of AI on performance.

分析识别出七项能显著放大或解锁 AI 收益的能力。AI 的价值由其周围的技术与文化环境所释放。这七大基础能力——横跨技术与文化两大领域——已被证明能放大 AI 对绩效的正面影响。

**1. A clear and communicated AI stance / 清晰且充分传达的 AI 立场**

A clear and communicated stance amplifies AI adoption's positive influence on individual effectiveness, organizational performance, and software delivery throughput. Organizations should clearly communicate their AI usage policies.

清晰且充分传达的 AI 立场能放大 AI 采用对个人效能、组织绩效和软件交付吞吐量的正面影响。组织应清晰传达其 AI 使用政策。

**2. Healthy data ecosystems / 健康的数据生态系统**

A healthy data ecosystem is foundational. Connect AI to your internal data: as AI adoption grows, providing AI access to quality internal data amplifies its benefits.

健康的数据生态系统是根基。将 AI 接入内部数据：随着 AI 采用的增长，让 AI 访问高质量内部数据能放大其收益。

**3. Strong version control practices / 强健的版本控制实践**

With the increased volume and velocity of code generation from AI, strong version control practices are more crucial than ever. Our research shows a powerful connection between mature version control habits and AI adoption. Specifically, frequent commits amplify AI's positive influence on individual effectiveness, while the frequent use of rollback features boosts the performance of AI-assisted teams.

随着 AI 生成代码的数量与速度增加，强健的版本控制实践比以往任何时候都更为关键。研究表明，成熟的版本控制习惯与 AI 采用之间存在强力关联。具体而言，频繁提交(commit)能放大 AI 对个人效能的正面影响，而频繁使用回滚(rollback)功能则能提升 AI 辅助团队的绩效。

The growing use of generative AI amplifies the importance of strong version control practices. AI-assisted coding can increase throughput, but our research shows it is also associated with increased software delivery instability. Strong version control practices are the essential safety net that allows teams to experiment with AI-generated code confidently.

生成式 AI 的日益普及放大了强健版本控制实践的重要性。AI 辅助编码可提升吞吐量，但研究表明它也与软件交付不稳定度上升相关。强健的版本控制实践是关键的安全网，使团队能够放心地试验 AI 生成的代码。

**4. Working in small batches / 小批量工作**

Working in small batches is a critical countermeasure to the risks of AI-assisted development. While AI can generate large amounts of code quickly, large changes are difficult to review, test, and integrate safely. Working in small batches increases reported product performance, while also decreasing perceived friction for AI-assisted teams.

小批量工作是应对 AI 辅助开发风险的关键对策。虽然 AI 能快速生成大量代码，但大型变更难以安全地 review（代码审查）、测试和集成。小批量工作能提升报告的产品绩效，同时降低 AI 辅助团队所感知的摩擦。

While AI can increase perceptions of individual effectiveness by generating large amounts of code, our findings show this isn't necessarily the most important metric. Instead, focus on outcomes. Enforce the discipline of working in small batches, which improves product performance and reduces friction for AI-assisted teams.

虽然 AI 能通过生成大量代码提升个人效能感，但研究显示这未必是最重要的指标。应聚焦于产出成果。贯彻小批量工作的纪律，这能改善产品绩效并降低 AI 辅助团队的摩擦。

**5. A user-centric focus / 以用户为中心**

A deep focus on the end-user's experience is paramount for teams utilizing AI. Our findings show that a user-centric focus amplifies the positive influence of AI on team performance. Importantly, we also found that in the absence of a user-centric focus, AI adoption can have a negative impact on team performance. When users are at the center of strategy, AI can help propel teams in the right direction. But, when users aren't the focus, AI-assisted development teams may just be moving quickly in the wrong direction.

对终端用户体验的深度关注对使用 AI 的团队至关重要。研究发现，以用户为中心能放大 AI 对团队绩效的正面影响。重要的是，我们还发现在缺乏以用户为中心的情况下，AI 采用可能对团队绩效产生负面影响。当用户处于战略核心时，AI 能助推团队朝正确方向前进；但当用户不被关注时，AI 辅助开发团队可能只是在错误方向上快速移动。

**6. A high-quality internal platform / 高质量内部平台**

Platforms are crucial for scaling success: 90% of organizations have already adopted internal platforms, and 76% now have dedicated platform teams to manage them, highlighting their importance in turning individual productivity gains from AI into systemic, organizational improvements.

平台对规模化成功至关重要：90% 的组织已采用内部平台，76% 拥有专职平台团队进行管理，这凸显了平台在将 AI 带来的个人生产力提升转化为系统性组织改进方面的重要性。

**7. AI access to internal data / AI 接入内部数据**

Providing AI access to internal data amplifies the benefits, as the platform provides the capabilities that allow AI to be maximally effective. Connect AI to your internal performance and reduce friction.

让 AI 接入内部数据能放大收益，因为平台提供了使 AI 发挥最大效用的能力。将 AI 接入内部绩效数据，并降低摩擦。

### Strategic Guidance / 战略指导

DORA's research has long held that even the best tools and teams can't succeed without the right organizational conditions. The findings of our inaugural DORA AI Capabilities Model are a reminder of this fact and suggest that successful AI-assisted development isn't just a purchasing decision; it's a decision to cultivate the conditions where AI-assisted developers thrive.

DORA 研究长期主张：即便是最优秀的工具和团队，缺乏合适的组织条件也无法成功。首届 DORA AI 能力模型的发现再次印证了这一点，表明成功的 AI 辅助开发不仅仅是采购决策，更是培育 AI 辅助开发者茁壮成长环境的决策。

Investing in these seven capabilities is an important step toward creating an environment where AI-assisted software development succeeds, leading to enhanced outcomes for your developers, your products, and your entire organization. Success with AI depends more on culture and capabilities than on the tools themselves.

投资这七项能力是迈向构建 AI 辅助软件开发成功环境的重要一步，能为开发者、产品和整个组织带来更优成果。AI 的成功更依赖于文化与能力，而非工具本身。

A team that wants to improve its product performance while using AI should focus on having accessible internal data, working in small batches, and clarifying its AI stance. AI is an amplifier: while almost 90% are using AI, our respondents say the greatest returns come from investing in foundational systems.

希望在使用 AI 的同时改善产品绩效的团队，应聚焦于让内部数据可访问、小批量工作，以及明确其 AI 立场。AI 是放大器：虽然近 90% 的人在使用 AI，但受访者表示最大回报来自对基础系统的投资。

### Research Methodology / 研究方法

Drawing on qualitative data and a global survey conducted between June 13 and July 21, 2025, this report uncovers several key findings on the state of AI-assisted software development. The research includes more than 100 hours of qualitative data and survey responses from nearly 5,000 technology professionals from around the world.

本报告基于 2025 年 6 月 13 日至 7 月 21 日期间开展的定性数据与全球问卷调查，揭示了 AI 辅助软件开发现状的多项关键发现。研究包含超过 100 小时的定性数据，以及来自全球近 5,000 名技术专业人士的问卷调查反馈。

### Companion Report / 配套报告

The DORA AI Capabilities Model report, a companion guide to the 2025 State of AI-assisted Software Development report, serves as a practical guide to the seven capabilities that amplify the benefits of AI. For each of the seven core capabilities, this report details implementation strategies, specific tactics for teams to get started, and methods for monitoring progress and fostering continuous improvement.

DORA AI 能力模型报告是《2025 AI 辅助软件开发现状》报告的配套指南，作为放大 AI 收益的七项能力的实用指南。针对七项核心能力中的每一项，该报告详述了实施策略、团队上手的具体战术，以及监控进展和促进持续改进的方法。

### Research Partners / 研究合作伙伴

The 2025 DORA Report is presented by Google Cloud in collaboration with the following research partners. Premier Research Partner: IT Revolution. Research Partners: GitHub, GitLab, SkillBench, Workhelix.

2025 DORA 报告由 Google Cloud 联合以下研究合作伙伴发布。首席研究合作伙伴：IT Revolution。研究合作伙伴：GitHub、GitLab、SkillBench、Workhelix。

IT Revolution empowers enterprise technology leaders with essential insights for succeeding in the digital age through books, papers, events, and more. GitHub is the world's leading AI-powered developer platform. GitLab is the most comprehensive, intelligent DevSecOps platform for software innovation. SkillBench helps organizations implement generative AI to boost productivity and skills. Workhelix helps large enterprises plan, co-create, measure, and accelerate AI transformation.

IT Revolution 通过书籍、论文、活动等形式，为企业技术领导者提供在数字时代取得成功的关键洞察。GitHub 是全球领先的 AI 驱动开发者平台。GitLab 是最全面的智能化 DevSecOps 软件创新平台。SkillBench 帮助组织实施生成式 AI 以提升生产力与技能。Workhelix 帮助大型企业规划、共创、衡量并加速 AI 转型。


---

## OWASP Top 10

原文链接：https://owasp.org/www-project-top-ten/

### OWASP Top 10 for Large Language Model Applications

This is the repository for the **OWASP Top 10 for Large Language Model Applications**. However, this project has now grown into the comprehensive **OWASP GenAI Security Project** - a global initiative that encompasses multiple security initiatives beyond just the Top 10 list.

这是 **OWASP 大语言模型应用十大安全风险** 的代码仓库。不过,该项目如今已发展为综合性的 **OWASP 生成式 AI 安全项目** —— 一项全球性倡议,涵盖的不仅是这份 Top 10 清单,还包含多个安全相关子项目。

### OWASP GenAI Security Project

The OWASP GenAI Security Project is a global, open-source initiative dedicated to identifying, mitigating, and documenting security and safety risks associated with generative AI technologies, including large language models (LLMs), agentic AI systems, and AI-driven applications. Our mission is to empower organizations, security professionals, AI practitioners, and policymakers with comprehensive, actionable guidance and tools to ensure the secure development, deployment, and governance of generative AI systems.

OWASP 生成式 AI 安全项目是一项全球性、开源的倡议,致力于识别、缓解并记录与生成式 AI 技术相关的安全与可靠性风险,这些技术涵盖大语言模型(LLM)、agentic AI(智能体式 AI)系统,以及 AI 驱动的应用。我们的使命是为组织、安全专业人员、AI 从业者和政策制定者提供全面、可操作的指南与工具,以确保生成式 AI 系统在开发、部署和治理层面安全可控。

**Learn more about our mission and charter:** [Project Mission and Charter](https://genai.owasp.org/project-mission-and-charter/)

**Visit our main project site:** [genai.owasp.org](https://genai.owasp.org)

**进一步了解我们的使命与章程:** [项目使命与章程](https://genai.owasp.org/project-mission-and-charter/)

**访问主项目站点:** [genai.owasp.org](https://genai.owasp.org)

### Latest Top 10 for LLM Applications

The OWASP Top 10 for Large Language Model Applications continues to be a core component of our work, identifying the most critical security vulnerabilities in LLM applications.

**Access the latest Top 10 for LLM:** [https://genai.owasp.org/llm-top-10/](https://genai.owasp.org/llm-top-10/)

### 最新版 LLM 应用 Top 10

OWASP 大语言模型应用十大安全风险仍是我们工作的核心组成部分,用于识别 LLM 应用中最为关键的安全漏洞。

**获取最新版 LLM Top 10:** [https://genai.owasp.org/llm-top-10/](https://genai.owasp.org/llm-top-10/)

### Project Background and Growth

The project has evolved significantly since its inception. From a small group of security professionals addressing an urgent security gap in 2023, it has grown into a global community with over 600 contributing experts from more than 18 countries and nearly 8,000 active community members.

**Read our full project background:** [Introduction and Background](https://genai.owasp.org/introduction-genai-security-project/)

### 项目背景与发展

自创立以来,该项目发生了显著演变。它最初只是 2023 年一小群安全专业人员为填补迫在眉睫的安全空白而成立,如今已发展为一个全球社区,拥有来自 18 个以上国家的 600 多名贡献专家,以及近 8000 名活跃的社区成员。

**阅读完整项目背景:** [介绍与背景](https://genai.owasp.org/introduction-genai-security-project/)

### Get Involved

#### Contribute to the Project

We welcome all expert ideas, contributions, suggestions, and remarks from security professionals, researchers, developers, and anyone passionate about AI security.

**Learn how to contribute:** [https://genai.owasp.org/contribute/](https://genai.owasp.org/contribute/)

### 参与其中

#### 为项目做贡献

我们欢迎来自安全专业人员、研究人员、开发者,以及任何对 AI 安全怀有热情之人的专家见解、贡献、建议与意见。

**了解如何贡献:** [https://genai.owasp.org/contribute/](https://genai.owasp.org/contribute/)

#### Join Our Meetings

Participate in our bi-weekly sync meetings and stay connected with the community.

**Meeting information:** [https://genai.owasp.org/meetings/](https://genai.owasp.org/meetings/)

#### 参加我们的会议

参加我们的双周同步会议,与社区保持联系。

**会议信息:** [https://genai.owasp.org/meetings/](https://genai.owasp.org/meetings/)

#### Connect with the Community

-   Join our working group channel on the [OWASP Slack](https://owasp.org/slack/invite) - sign up and join us on the `#project-top10-for-llm` channel
-   [Follow our project LinkedIn page](https://www.linkedin.com/company/owasp-top-10-for-large-language-model-applications/)
-   [Subscribe to our newsletter](https://llmtop10.beehiiv.com/subscribe) for periodic updates

#### 与社区保持联系

-   加入我们在 [OWASP Slack](https://owasp.org/slack/invite) 上的工作组频道 —— 注册后在 `#project-top10-for-llm` 频道与我们汇合
-   [关注我们项目的 LinkedIn 主页](https://www.linkedin.com/company/owasp-top-10-for-large-language-model-applications/)
-   [订阅我们的简报](https://llmtop10.beehiiv.com/subscribe) 以获取定期更新

### Project Support

We are a not-for-profit, open-source, community-driven project. If you are interested in supporting the project with resources or becoming a sponsor to help us sustain community efforts and offset operational and outreach costs, visit the [Sponsor Section](https://genai.owasp.org/sponsorship) on our website.

**Thank you to our current [Sponsors and Supporters](https://genai.owasp.org/supporters/)**

### 项目支持

我们是一个非营利、开源、社区驱动的项目。若你有意以资源支持本项目,或希望成为赞助商以帮助我们维系社区工作、抵消运营与外联成本,请访问我们网站上的 [赞助专区](https://genai.owasp.org/sponsorship)。

**感谢我们现有的 [赞助商与支持者](https://genai.owasp.org/supporters/)**

### Educational Resources

New to LLM Application security? Check out our [resources page](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/wiki/Educational-Resources) to learn more.

### 教育资源

刚接触 LLM 应用安全?请查看我们的 [资源页面](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/wiki/Educational-Resources) 以了解更多。

---

### OWASP Top 10 for Large Language Model Applications version 1.1

### OWASP 大语言模型应用十大安全风险 版本 1.1

### LLM01: Prompt Injection

Manipulating LLMs via crafted inputs can lead to unauthorized access, data breaches, and compromised decision-making.

### LLM01:Prompt 注入(提示词注入)

通过精心构造的输入对 LLM 进行操纵,可能导致未授权访问、数据泄露,以及决策机制被破坏。

### LLM02: Insecure Output Handling

Neglecting to validate LLM outputs may lead to downstream security exploits, including code execution that compromises systems and exposes data.

### LLM02:不安全的输出处理

若不对 LLM 的输出进行校验,可能引发下游安全漏洞利用,包括代码执行,从而危及系统并暴露数据。

### LLM03: Training Data Poisoning

Tampered training data can impair LLM models leading to responses that may compromise security, accuracy, or ethical behavior.

### LLM03:训练数据投毒

被篡改的训练数据会损害 LLM 模型,使其产生可能破坏安全性、准确性或伦理行为的响应。

### LLM04: Model Denial of Service

Overloading LLMs with resource-heavy operations can cause service disruptions and increased costs.

### LLM04:模型拒绝服务

以资源密集型操作让 LLM 过载,可造成服务中断与成本攀升。

### LLM05: Supply Chain Vulnerabilities

Depending upon compromised components, services or datasets undermine system integrity, causing data breaches and system failures.

### LLM05:供应链漏洞

依赖受损的组件、服务或数据集会破坏系统完整性,导致数据泄露与系统故障。

### LLM06: Sensitive Information Disclosure

Failure to protect against disclosure of sensitive information in LLM outputs can result in legal consequences or a loss of competitive advantage.

### LLM06:敏感信息泄露

未能防护 LLM 输出中敏感信息的泄露,可能招致法律后果或丧失竞争优势。

### LLM07: Insecure Plugin Design

LLM plugins processing untrusted inputs and having insufficient access control risk severe exploits like remote code execution.

### LLM07:不安全的插件设计

LLM 插件若处理不可信输入且访问控制不足,将面临远程代码执行等严重利用风险。

### LLM08: Excessive Agency

Granting LLMs unchecked autonomy to take action can lead to unintended consequences, jeopardizing reliability, privacy, and trust.

### LLM08:过度代理权限

授予 LLM 不受约束的自主行动权,可能产生非预期后果,危及可靠性、隐私与信任。

### LLM09: Overreliance

Failing to critically assess LLM outputs can lead to compromised decision making, security vulnerabilities, and legal liabilities.

### LLM09:过度依赖

未能批判性评估 LLM 输出,可能导致决策失误、安全漏洞,以及法律责任。

### LLM10: Model Theft

Unauthorized access to proprietary large language models risks theft, competitive advantage, and dissemination of sensitive information.

### LLM10:模型窃取

对专有大语言模型的未授权访问,可能造成模型被盗、竞争优势丧失,以及敏感信息扩散。

---

### Top 10 for Large Language Model Applications Information

-   [Lab Status Project](https://owasp.org/projects/)
-   [Version 2025](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/)
-   [Version 1.1.0 Translations](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/tree/main/assets/translations) (archived)
-   [Version 1.1.0](assets/PDF/OWASP-Top-10-for-LLMs-2023-v1_1.pdf) (archived)
-   [Version 1.0.1](assets/PDF/OWASP-Top-10-for-LLMs-2023-v1_0_1.pdf) (archived)
-   [Version 1.0.0](assets/PDF/OWASP-Top-10-for-LLMs-2023-v1_0.pdf) (archived)
-   [Version 0.9.0](assets/PDF/OWASP-Top-10-for-LLMs-2023-v09.pdf) (archived)
-   [Version 0.5.0](assets/PDF/OWASP-Top-10-for-LLMs-2023-v05.pdf) (archived)
-   [Version 0.1.0](Archive/0_1_vulns/) (archived)

### 大语言模型应用 Top 10 相关信息

-   [实验室状态项目](https://owasp.org/projects/)
-   [2025 版](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/)
-   [1.1.0 版翻译](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/tree/main/assets/translations)(已归档)
-   [1.1.0 版](assets/PDF/OWASP-Top-10-for-LLMs-2023-v1_1.pdf)(已归档)
-   [1.0.1 版](assets/PDF/OWASP-Top-10-for-LLMs-2023-v1_0_1.pdf)(已归档)
-   [1.0.0 版](assets/PDF/OWASP-Top-10-for-LLMs-2023-v1_0.pdf)(已归档)
-   [0.9.0 版](assets/PDF/OWASP-Top-10-for-LLMs-2023-v09.pdf)(已归档)
-   [0.5.0 版](assets/PDF/OWASP-Top-10-for-LLMs-2023-v05.pdf)(已归档)
-   [0.1.0 版](Archive/0_1_vulns/)(已归档)

### Social Links

-   [Subscribe to our Newsletter](https://llmtop10.beehiiv.com/subscribe)
-   [v1.1 Announcement](https://www.linkedin.com/pulse/new-release-owasp-top-10-llm-apps-steve-wilson?trk=public_post_feed-article-content)
-   [v1 Announcement](https://www.linkedin.com/pulse/official-release-owasp-top-10-large-language-model-v10-steve-wilson/)
-   [Project Announcement](https://www.linkedin.com/pulse/announcing-owasp-top-10-large-language-models-ai-project-steve-wilson/)
-   [Share on Twitter](https://twitter.com/intent/tweet?url=https://owasp.org/www-project-top-10-for-large-language-model-applications/&text=Check%20out%20the%20OWASP%20Top%2010%20for%20Large%20Language%20Model%20Applications%20project:%20)
-   [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https://owasp.org/www-project-top-10-for-large-language-model-applications/)

### 社交链接

-   [订阅我们的简报](https://llmtop10.beehiiv.com/subscribe)
-   [v1.1 发布公告](https://www.linkedin.com/pulse/new-release-owasp-top-10-llm-apps-steve-wilson?trk=public_post_feed-article-content)
-   [v1 发布公告](https://www.linkedin.com/pulse/official-release-owasp-top-10-large-language-model-v10-steve-wilson/)
-   [项目发布公告](https://www.linkedin.com/pulse/announcing-owasp-top-10-large-language-models-ai-project-steve-wilson/)
-   [在 Twitter 上分享](https://twitter.com/intent/tweet?url=https://owasp.org/www-project-top-10-for-large-language-model-applications/&text=Check%20out%20the%20OWASP%20Top%2010%20for%20Large%20Language%20Model%20Applications%20project:%20)
-   [在 LinkedIn 上分享](https://www.linkedin.com/sharing/share-offsite/?url=https://owasp.org/www-project-top-10-for-large-language-model-applications/)

### Code Repository

-   [repo](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications)
-   [wiki](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/wiki)

### Change Log

-   [changes](changes)

### 代码仓库

-   [仓库](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications)
-   [wiki](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/wiki)

### 变更日志

-   [变更记录](changes)

### Leaders

-   Lead [Steve Wilson](/cdn-cgi/l/email-protection#6c1f18091a09421b05001f03022c031b0d1f1c42031e0b) - [LinkedIn](https://www.linkedin.com/in/wilsonsd/) [Twitter](https://twitter.com/virtualsteve)
-   Co-lead [Ads Dawson](/cdn-cgi/l/email-protection#d7b6b3a4f9b3b6a0a4b8b997b8a0b6a4a7f9b8a5b0) - [LinkedIn](https://www.linkedin.com/in/adamdawson0/) [GitHub](https://github.com/GangGreenTemperTatum)
-   Co-lead [John Sotiropoulos](/cdn-cgi/l/email-protection#ed87828583c39e8299849f829d829881829ead829a8c9e9dc3829f8a) - [LinkedIn](https://www.linkedin.com/in/jsotiropoulos/)
-   Co-lead [Scott Clinton](/cdn-cgi/l/email-protection#2152424e55550f424d484f554e4f614e564052510f4e5346) - [LinkedIn](https://www.linkedin.com/in/scottjclinton/)
-   Co-lead [Sandy Dunn](/cdn-cgi/l/email-protection#bccfddd2d8c592d8c9d2d2fcd3cbddcfcc92d3cedb) - [LinkedIn](https://www.linkedin.com/in/sandydunnciso/)

### 项目负责人

-   负责人 [Steve Wilson](/cdn-cgi/l/email-protection#6c1f18091a09421b05001f03022c031b0d1f1c42031e0b) - [LinkedIn](https://www.linkedin.com/in/wilsonsd/) [Twitter](https://twitter.com/virtualsteve)
-   联合负责人 [Ads Dawson](/cdn-cgi/l/email-protection#d7b6b3a4f9b3b6a0a4b8b997b8a0b6a4a7f9b8a5b0) - [LinkedIn](https://www.linkedin.com/in/adamdawson0/) [GitHub](https://github.com/GangGreenTemperTatum)
-   联合负责人 [John Sotiropoulos](/cdn-cgi/l/email-protection#ed87828583c39e8299849f829d829881829ead829a8c9e9dc3829f8a) - [LinkedIn](https://www.linkedin.com/in/jsotiropoulos/)
-   联合负责人 [Scott Clinton](/cdn-cgi/l/email-protection#2152424e55550f424d484f554e4f614e564052510f4e5346) - [LinkedIn](https://www.linkedin.com/in/scottjclinton/)
-   联合负责人 [Sandy Dunn](/cdn-cgi/l/email-protection#bccfddd2d8c592d8c9d2d2fcd3cbddcfcc92d3cedb) - [LinkedIn](https://www.linkedin.com/in/sandydunnciso/)

### Core Leadership vTeam

-   Full Core Team [Team Page](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/wiki/Core-Team)

### 核心领导虚拟团队

-   完整核心团队 [团队页面](https://github.com/OWASP/www-project-top-10-for-large-language-model-applications/wiki/Core-Team)

### Corporate Supporters

[Become a corporate supporter](https://owasp.org/supporters)

### 企业支持者

[成为企业支持者](https://owasp.org/supporters)

---

The OWASP® Foundation works to improve the security of software through its community-led open source software projects, hundreds of chapters worldwide, tens of thousands of members, and by hosting local and global conferences.

OWASP® 基金会通过社区主导的开源软件项目、全球数以百计的分会、数以万计的会员,以及举办本地与全球会议,致力于提升软件安全性。

OWASP, the OWASP logo, and Global AppSec are registered trademarks and AppSec Days, AppSec California, AppSec Cali, SnowFROC, OWASP Boston Application Security Conference, and LASCON are trademarks of the OWASP Foundation, Inc. Unless otherwise specified, all content on the site is Creative Commons Attribution-ShareAlike v4.0 and provided without warranty of service or accuracy. For more information, please refer to our [General Disclaimer](https://policy.owasp.org/operational/general-disclaimer.html). OWASP does not endorse or recommend commercial products or services, allowing our community to remain vendor neutral with the collective wisdom of the best minds in software security worldwide. Copyright 2025, OWASP Foundation, Inc.

OWASP、OWASP 徽标以及 Global AppSec 均为注册商标;AppSec Days、AppSec California、AppSec Cali、SnowFROC、OWASP Boston Application Security Conference 与 LASCON 为 OWASP Foundation, Inc. 的商标。除非另有说明,本站所有内容均采用知识共享 署名-相同方式共享 v4.0 许可,且不对其服务可用性或准确性作任何担保。更多信息请参阅我们的 [通用免责声明](https://policy.owasp.org/operational/general-disclaimer.html)。OWASP 不背书或推荐任何商业产品或服务,以使我们的社区保持厂商中立,汇聚全球软件安全领域顶尖人才的集体智慧。版权所有 2025,OWASP Foundation, Inc.

---

## GitHub Copilot - Repository Instructions（仓库指令）

原文链接：https://docs.github.com/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot


### Adding repository custom instructions for GitHub Copilot

Create repository custom instructions files that give Copilot additional context on how to understand your project and how to build, test and validate its changes.

为 GitHub Copilot 添加仓库自定义指令（repository custom instructions）。通过创建仓库自定义指令文件，向 Copilot 提供额外上下文，帮助其理解你的项目，以及如何构建、测试和验证其改动。

### Introduction

Repository custom instructions let you provide Copilot with repository-specific guidance and preferences on GitHub. To find out how to set up custom instructions in an IDE, see [Adding repository custom instructions for GitHub Copilot in your IDE](/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide). For more information about custom instructions, see [About customizing GitHub Copilot responses](/en/copilot/concepts/prompting/response-customization).

### 简介

仓库自定义指令允许你在 GitHub 上向 Copilot 提供针对特定仓库的指引和偏好。若要了解如何在 IDE 中设置自定义指令，请参阅 [Adding repository custom instructions for GitHub Copilot in your IDE](/en/copilot/how-tos/configure-custom-instructions-in-your-ide/add-repository-instructions-in-your-ide)。有关自定义指令的更多信息，请参阅 [About customizing GitHub Copilot responses](/en/copilot/concepts/prompting/response-customization)。

### Prerequisites for repository custom instructions

* You must have a custom instructions file (see the instructions below).

* For Copilot code review, your personal choice of whether to use custom instructions must be set to enabled. This is enabled by default. See [Enabling or disabling repository custom instructions](#enabling-or-disabling-custom-instructions-for-copilot-code-review) later in this article.

### 仓库自定义指令的前置条件

* 你必须拥有一个自定义指令文件（参见下方说明）。

* 对于 Copilot code review（代码审查），你个人关于是否使用自定义指令的选项必须设为启用。该选项默认启用。请参阅本文稍后的 [Enabling or disabling repository custom instructions](#enabling-or-disabling-custom-instructions-for-copilot-code-review)。

### Creating custom instructions

Copilot on GitHub supports three types of repository custom instructions. For details of which GitHub Copilot features support these types of instructions, see [About customizing GitHub Copilot responses](/en/copilot/concepts/prompting/response-customization?tool=webui#support-for-repository-custom-instructions).

### 创建自定义指令

GitHub 上的 Copilot 支持三种类型的仓库自定义指令。关于哪些 GitHub Copilot 功能支持这些指令类型的详细信息，请参阅 [About customizing GitHub Copilot responses](/en/copilot/concepts/prompting/response-customization?tool=webui#support-for-repository-custom-instructions)。

* **Repository-wide custom instructions** apply to all requests made in the context of a repository.

  These are specified in a `copilot-instructions.md` file in the `.github` directory of the repository. See [Creating repository-wide custom instructions](#creating-repository-wide-custom-instructions).

* **仓库级自定义指令**（Repository-wide custom instructions）适用于在某个仓库上下文中发起的所有请求。

  这类指令通过仓库 `.github` 目录下的 `copilot-instructions.md` 文件指定。参见 [Creating repository-wide custom instructions](#creating-repository-wide-custom-instructions)。

* **Path-specific custom instructions** apply to requests made in the context of files that match a specified path.

  These are specified in one or more `NAME.instructions.md` files within or below the `.github/instructions` directory in the repository. See [Creating path-specific custom instructions](#creating-path-specific-custom-instructions).

  If the path you specify matches a file that Copilot is working on, and a repository-wide custom instructions file also exists, then the instructions from both files are used.

* **路径特定自定义指令**（Path-specific custom instructions）适用于在匹配指定路径的文件上下文中发起的请求。

  这类指令通过仓库 `.github/instructions` 目录内或其子目录下的一个或多个 `NAME.instructions.md` 文件指定。参见 [Creating path-specific custom instructions](#creating-path-specific-custom-instructions)。

  如果你指定的路径与 Copilot 正在处理的文件相匹配，且同时存在仓库级自定义指令文件，则两个文件中的指令都会被使用。

* **Agent instructions** are used by AI agents.

  You can create one or more `AGENTS.md` files, stored anywhere within the repository. When Copilot is working, the nearest `AGENTS.md` file in the directory tree will take precedence. For more information, see the [agentsmd/agents.md repository](https://github.com/agentsmd/agents.md).

  Alternatively, you can use a single `CLAUDE.md` or `GEMINI.md` file stored in the root of the repository.

* **Agent 指令**（Agent instructions，供 AI 代理使用）由 AI agent（智能代理）使用。

  你可以创建一个或多个 `AGENTS.md` 文件，存放在仓库内的任意位置。当 Copilot 工作时，目录树中距离最近的 `AGENTS.md` 文件将优先生效。更多信息请参阅 [agentsmd/agents.md 仓库](https://github.com/agentsmd/agents.md)。

  或者，你也可以使用存放在仓库根目录下的单个 `CLAUDE.md` 或 `GEMINI.md` 文件。

### Creating repository-wide custom instructions

You can create your own custom instructions file from scratch. See [Writing your own copilot-instructions.md file](#writing-your-own-copilot-instructionsmd-file). Alternatively, you can ask Copilot cloud agent to generate one for you.

### 创建仓库级自定义指令

你可以从零开始创建自己的自定义指令文件，参见 [Writing your own copilot-instructions.md file](#writing-your-own-copilot-instructionsmd-file)。或者，你也可以让 Copilot cloud agent（云端代理）为你生成一个。

### Asking Copilot cloud agent to generate a `copilot-instructions.md` file

1. Go to the agents tab at [github.com/copilot/agents](https://github.com/copilot/agents?ref_product=copilot&ref_type=engagement&ref_style=text).

   You can also reach this page by clicking the **Copilot** button next to the search bar on any page on GitHub, then selecting **Agents** from the sidebar.

### 让 Copilot cloud agent 生成 `copilot-instructions.md` 文件

1. 前往 agents 标签页：[github.com/copilot/agents](https://github.com/copilot/agents?ref_product=copilot&ref_type=engagement&ref_style=text)。

   你也可以在 GitHub 任意页面点击搜索栏旁边的 **Copilot** 按钮，然后从侧边栏选择 **Agents** 到达此页面。

2. In the prompt field dropdown, select the repository you want Copilot to generate custom instructions for.

3. Copy the following prompt and paste it into the prompt field, customizing it if needed:

2. 在 prompt（提示词）输入框的下拉菜单中，选择你希望 Copilot 为其生成自定义指令的仓库。

3. 复制以下 prompt 并粘贴到输入框中，如有需要可自行修改：

   ```markdown copy
   Your task is to "onboard" this repository to Copilot cloud agent by adding a .github/copilot-instructions.md file in the repository that contains information describing how a cloud agent seeing it for the first time can work most efficiently.

   You will do this task only one time per repository and doing a good job can SIGNIFICANTLY improve the quality of the agent's work, so take your time, think carefully, and search thoroughly before writing the instructions.

   <Goals>
   - Reduce the likelihood of a cloud agent pull request getting rejected by the user due to
   generating code that fails the continuous integration build, fails a validation pipeline, or
   having misbehavior.
   - Minimize bash command and build failures.
   - Allow the agent to complete its task more quickly by minimizing the need for exploration using grep, find, str_replace_editor, and code search tools.
   </Goals>

   <Limitations>
   - Instructions must be no longer than 2 pages.
   - Instructions must not be task specific.
   </Limitations>

   <WhatToAdd>

   Add the following high level details about the codebase to reduce the amount of searching the agent has to do to understand the codebase each time:
   <HighLevelDetails>

   - A summary of what the repository does.
   - High level repository information, such as the size of the repo, the type of the project, the languages, frameworks, or target runtimes in use.
   </HighLevelDetails>

   Add information about how to build and validate changes so the agent does not need to search and find it each time.
   <BuildInstructions>

   - For each of bootstrap, build, test, run, lint, and any other scripted step, document the sequence of steps to take to run it successfully as well as the versions of any runtime or build tools used.
   - Each command should be validated by running it to ensure that it works correctly as well as any preconditions and postconditions.
   - Try cleaning the repo and environment and running commands in different orders and document errors and misbehavior observed as well as any steps used to mitigate the problem.
   - Run the tests and document the order of steps required to run the tests.
   - Make a change to the codebase. Document any unexpected build issues as well as the workarounds.
   - Document environment setup steps that seem optional but that you have validated are actually required.
   - Document the time required for commands that failed due to timing out.
   - When you find a sequence of commands that work for a particular purpose, document them in detail.
   - Use language to indicate when something should always be done. For example: "always run npm install before building".
   - Record any validation steps from documentation.
   </BuildInstructions>

   List key facts about the layout and architecture of the codebase to help the agent find where to make changes with minimal searching.
   <ProjectLayout>

   - A description of the major architectural elements of the project, including the relative paths to the main project files, the location
   of configuration files for linting, compilation, testing, and preferences.
   - A description of the checks run prior to check in, including any GitHub workflows, continuous integration builds, or other validation pipelines.
   - Document the steps so that the agent can replicate these itself.
   - Any explicit validation steps that the agent can consider to have further confidence in its changes.
   - Dependencies that aren't obvious from the layout or file structure.
   - Finally, fill in any remaining space with detailed lists of the following, in order of priority: the list of files in the repo root, the
   contents of the README, the contents of any key source files, the list of files in the next level down of directories, giving priority to the more structurally important and snippets of code from key source files, such as the one containing the main method.
   </ProjectLayout>
   </WhatToAdd>

   <StepsToFollow>
   - Perform a comprehensive inventory of the codebase. Search for and view:
   - README.md, CONTRIBUTING.md, and all other documentation files.
   - Search the codebase for build steps and indications of workarounds like 'HACK', 'TODO', etc.
   - All scripts, particularly those pertaining to build and repo or environment setup.
   - All build and actions pipelines.
   - All project files.
   - All configuration and linting files.
   - For each file:
   - think: are the contents or the existence of the file information that the cloud agent will need to implement, build, test, validate, or demo a code change?
   - If yes:
      - Document the command or information in detail.
      - Explicitly indicate which commands work and which do not and the order in which commands should be run.
      - Document any errors encountered as well as the steps taken to workaround them.
   - Document any other steps or information that the agent can use to reduce time spent exploring or trying and failing to run bash commands.
   - Finally, explicitly instruct the agent to trust the instructions and only perform a search if the information in the instructions is incomplete or found to be in error.
   </StepsToFollow>
      - Document any errors encountered as well as the steps taken to work-around them.

   ```

（以上为 prompt 原文，保持不翻译。）

4. Click **Start task** or press <kbd>Enter</kbd>.

Copilot will start a new session, which will appear in the list below the prompt box. Copilot will create a draft pull request（草稿拉取请求）, write your custom instructions, push them to the branch, then add you as a reviewer when finished, triggering a notification.

4. 点击 **Start task**（开始任务）按钮，或按 <kbd>Enter</kbd> 键。

Copilot 将启动一个新的会话，该会话会出现在输入框下方的列表中。Copilot 会创建一个 draft pull request（草稿拉取请求），写入你的自定义指令，将其推送到分支，完成后把你添加为 reviewer（审查者），并触发通知。

### Writing your own `copilot-instructions.md` file

1. In the root of your repository, create a file named `.github/copilot-instructions.md`.

   Create the `.github` directory if it does not already exist.

2. Add natural language instructions to the file, in Markdown format.

   Whitespace between instructions is ignored, so the instructions can be written as a single paragraph, each on a new line, or separated by blank lines for legibility.

### 自行编写 `copilot-instructions.md` 文件

1. 在仓库根目录下创建一个名为 `.github/copilot-instructions.md` 的文件。

   如果 `.github` 目录尚不存在，请先创建它。

2. 以 Markdown 格式向文件中添加自然语言指令。

   指令之间的空白符会被忽略，因此可以写成单个段落、每条指令独占一行，或用空行分隔以提升可读性。

> [!TIP]
> The first time you create a pull request in a given repository with Copilot cloud agent, Copilot will leave a comment with a link to automatically generate custom instructions for the repository.

> [!TIP]（提示）
> 当你首次使用 Copilot cloud agent 在某个仓库中创建 pull request 时，Copilot 会留下一条评论，其中包含一个链接，可自动为该仓库生成自定义指令。

### Creating path-specific custom instructions

> [!NOTE]
> Currently, on GitHub.com, path-specific custom instructions are only supported for Copilot cloud agent and Copilot code review.

### 创建路径特定自定义指令

> [!NOTE]（注意）
> 目前在 GitHub.com 上，路径特定自定义指令仅被 Copilot cloud agent 和 Copilot code review 支持。

1. Create the `.github/instructions` directory if it does not already exist.

2. Optionally, create subdirectories of `.github/instructions` to organize your instruction files.

3. Create one or more `NAME.instructions.md` files, where `NAME` indicates the purpose of the instructions. The file name must end with `.instructions.md`.

1. 如果 `.github/instructions` 目录尚不存在，请先创建它。

2. 可选：在 `.github/instructions` 下创建子目录，以便组织你的指令文件。

3. 创建一个或多个 `NAME.instructions.md` 文件，其中 `NAME` 表示指令的用途。文件名必须以 `.instructions.md` 结尾。

4. At the start of the file, create a frontmatter block containing the `applyTo` keyword. Use glob syntax to specify what files or directories the instructions apply to.

   For example:

   ```markdown
   ---
   applyTo: "app/models/**/*.rb"
   ---
   ```

4. 在文件开头创建一个 frontmatter（前置元数据）块，其中包含 `applyTo` 关键字。使用 glob 语法指定指令适用于哪些文件或目录。

   例如：

   ```markdown
   ---
   applyTo: "app/models/**/*.rb"
   ---
   ```

   You can specify multiple patterns by separating them with commas. For example, to apply the instructions to all TypeScript files in the repository, you could use the following frontmatter block:

   ```markdown
   ---
   applyTo: "**/*.ts,**/*.tsx"
   ---
   ```

   你可以通过逗号分隔来指定多个模式。例如，要让指令适用于仓库中的所有 TypeScript 文件，可以使用如下 frontmatter 块：

   ```markdown
   ---
   applyTo: "**/*.ts,**/*.tsx"
   ---
   ```

   Glob examples:

   * `*` - will all match all files in the current directory.
   * `**` or `**/*` - will all match all files in all directories.
   * `*.py` - will match all `.py` files in the current directory.
   * `**/*.py` - will recursively match all `.py` files in all directories.
   * `src/*.py` - will match all `.py` files in the `src` directory. For example, `src/foo.py` and `src/bar.py` but *not* `src/foo/bar.py`.
   * `src/**/*.py` - will recursively match all `.py` files in the `src` directory. For example, `src/foo.py`, `src/foo/bar.py`, and `src/foo/bar/baz.py`.
   * `**/subdir/**/*.py` - will recursively match all `.py` files in any `subdir` directory at any depth. For example, `subdir/foo.py`, `subdir/nested/bar.py`, `parent/subdir/baz.py`, and `deep/parent/subdir/nested/qux.py`, but *not* `foo.py` at a path that does not contain a `subdir` directory.

   Glob 示例：

   * `*` - 匹配当前目录下的所有文件。
   * `**` 或 `**/*` - 匹配所有目录下的所有文件。
   * `*.py` - 匹配当前目录下所有 `.py` 文件。
   * `**/*.py` - 递归匹配所有目录下的所有 `.py` 文件。
   * `src/*.py` - 匹配 `src` 目录下的所有 `.py` 文件。例如 `src/foo.py` 和 `src/bar.py`，但*不*匹配 `src/foo/bar.py`。
   * `src/**/*.py` - 递归匹配 `src` 目录下的所有 `.py` 文件。例如 `src/foo.py`、`src/foo/bar.py` 和 `src/foo/bar/baz.py`。
   * `**/subdir/**/*.py` - 递归匹配任意深度的 `subdir` 目录中的所有 `.py` 文件。例如 `subdir/foo.py`、`subdir/nested/bar.py`、`parent/subdir/baz.py` 和 `deep/parent/subdir/nested/qux.py`，但*不*匹配路径中不包含 `subdir` 目录的 `foo.py`。

5. Optionally, to prevent the file from being used by either Copilot cloud agent or Copilot code review, add the `excludeAgent` keyword to the frontmatter block. Use either `"code-review"` or `"cloud-agent"`.

   For example, the following file will only be read by Copilot cloud agent.

   ```markdown
   ---
   applyTo: "**"
   excludeAgent: "code-review"
   ---
   ```

5. 可选：若要阻止该文件被 Copilot cloud agent 或 Copilot code review 使用，可在 frontmatter 块中添加 `excludeAgent` 关键字。取值为 `"code-review"` 或 `"cloud-agent"`。

   例如，以下文件只会被 Copilot cloud agent 读取。

   ```markdown
   ---
   applyTo: "**"
   excludeAgent: "code-review"
   ---
   ```

   If the `excludeAgent` keyword is not included in the front matter block, both Copilot code review and Copilot cloud agent will use your instructions.

6. Add your custom instructions in natural language, using Markdown format. Whitespace between instructions is ignored, so the instructions can be written as a single paragraph, each on a new line, or separated by blank lines for legibility.

   如果 frontmatter 块中未包含 `excludeAgent` 关键字，则 Copilot code review 和 Copilot cloud agent 都会使用你的指令。

6. 以 Markdown 格式用自然语言添加自定义指令。指令之间的空白符会被忽略，因此可以写成单个段落、每条指令独占一行，或用空行分隔以提升可读性。

（此处原文含一个交互式反馈组件，含"是否成功添加自定义指令文件"的 Yes/No 按钮，此处省略。）

### Custom instructions in use

The instructions in the file(s) are available for use by Copilot as soon as you save the file(s). Instructions are automatically added to requests that you submit to Copilot.

### 自定义指令的使用

文件中的指令在你保存文件后即可供 Copilot 使用。指令会自动添加到你提交给 Copilot 的请求中。

In Copilot Chat ([github.com/copilot](https://github.com/copilot)), you can start a conversation that uses repository custom instructions by adding, as an attachment, the repository that contains the instructions file.

在 Copilot Chat（[github.com/copilot](https://github.com/copilot)）中，你可以将包含指令文件的仓库作为附件添加，从而发起一个使用仓库自定义指令的对话。

Whenever repository custom instructions are used by Copilot Chat, the instructions file is added as a reference for the response that's generated. To find out whether repository custom instructions were used, expand the list of references at the top of a chat response in the Chat panel and check whether the `.github/copilot-instructions.md` file is listed.

每当 Copilot Chat 使用仓库自定义指令时，指令文件会作为所生成回复的引用（reference）被添加。若要确认是否使用了仓库自定义指令，可在 Chat 面板中展开某条聊天回复顶部的引用列表，查看其中是否列出了 `.github/copilot-instructions.md` 文件。

![Screenshot of an expanded References list, showing the 'copilot-instructions.md' file highlighted with a dark orange outline.](/assets/images/help/copilot/custom-instructions-ref-in-github.png)

（图片说明：展开的 References 列表截图，其中 `copilot-instructions.md` 文件以深橙色边框高亮显示。）

You can click the reference to open the file.

你可以点击该引用以打开文件。

> [!NOTE]
>
> * Multiple types of custom instructions can apply to a request sent to Copilot. Personal instructions take the highest priority. Repository instructions come next, and then organization instructions are prioritized last. However, all sets of relevant instructions are provided to Copilot.
> * Whenever possible, try to avoid providing conflicting sets of instructions. If you are concerned about response quality, you can temporarily disable repository instructions. See [Adding repository custom instructions for GitHub Copilot](/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot?tool=webui#enabling-or-disabling-repository-custom-instructions).

> [!NOTE]（注意）
>
> * 多种类型的自定义指令可能同时适用于发送给 Copilot 的同一个请求。个人指令优先级最高，其次是仓库指令，组织指令优先级最低。不过，所有相关的指令集合都会被提供给 Copilot。
> * 尽可能避免提供相互冲突的指令集合。如果你担心回复质量，可以临时禁用仓库指令。参见 [Adding repository custom instructions for GitHub Copilot](/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot?tool=webui#enabling-or-disabling-repository-custom-instructions)。

### Enabling or disabling custom instructions for Copilot code review

Custom instructions are enabled for Copilot code review by default but you can disable, or re-enable, them in the repository settings on GitHub.com. This applies to Copilot's use of custom instructions for all code reviews it performs in this repository.

### 为 Copilot code review 启用或禁用自定义指令

Copilot code review 默认启用自定义指令，但你可以在 GitHub.com 的仓库设置中禁用或重新启用。此设置适用于 Copilot 在该仓库中执行的所有 code review 对自定义指令的使用。

1. On GitHub, navigate to the main page of the repository.
2. Under your repository name, click **Settings**. If you cannot see the "Settings" tab, select the **More** dropdown menu, then click **Settings**.

   ![Screenshot of a repository header showing the tabs. The "Settings" tab is highlighted by a dark orange outline.](/assets/images/help/repository/repo-actions-settings.png)

1. 在 GitHub 上，导航到仓库的主页。
2. 在仓库名称下方，点击 **Settings**（设置）。如果看不到"Settings"标签页，请选择 **More**（更多）下拉菜单，然后点击 **Settings**。

   （图片说明：仓库头部标签页截图，"Settings" 标签以深橙色边框高亮显示。）

3. In the "Code & automation" section of the sidebar, click **Copilot**, then **Code review**.
4. Toggle the "Use custom instructions when reviewing pull requests" option on or off.

3. 在侧边栏的 "Code & automation"（代码与自动化）部分，点击 **Copilot**，然后点击 **Code review**。
4. 将 "Use custom instructions when reviewing pull requests"（审查 pull request 时使用自定义指令）选项打开或关闭。

> [!NOTE]
> When reviewing a pull request, Copilot uses the custom instructions in the base branch of the pull request. For example, if your pull request seeks to merge `my-feature-branch` into `main`, Copilot will use the custom instructions in `main`.

> [!NOTE]（注意）
> 审查 pull request 时，Copilot 使用该 pull request 基分支（base branch）中的自定义指令。例如，如果你的 pull request 是将 `my-feature-branch` 合并到 `main`，Copilot 将使用 `main` 中的自定义指令。

### Further reading

* [Support for different types of custom instructions](/en/copilot/reference/custom-instructions-support)
* [Custom instructions](/en/copilot/tutorials/customization-library/custom-instructions)—a curated collection of examples
* [Using custom instructions to unlock the power of Copilot code review](/en/copilot/tutorials/use-custom-instructions)

### 延伸阅读

* [Support for different types of custom instructions](/en/copilot/reference/custom-instructions-support)（不同类型自定义指令的支持情况）
* [Custom instructions](/en/copilot/tutorials/customization-library/custom-instructions)（自定义指令）— 精选示例合集
* [Using custom instructions to unlock the power of Copilot code review](/en/copilot/tutorials/use-custom-instructions)（使用自定义指令释放 Copilot code review 的潜力）

---

## GitHub Copilot - Code Review（代码审查）

原文链接：https://docs.github.com/en/copilot/using-github-copilot/code-review

### Using GitHub Copilot code review

Learn how to request a code review (代码审查) from GitHub Copilot.

了解如何向 GitHub Copilot 请求 code review（代码审查）。

## Introduction

GitHub Copilot can review your code and provide feedback. Where possible, Copilot's feedback includes suggested changes which you can apply with a couple of clicks.

GitHub Copilot 可以审查你的代码并提供反馈。在可能的情况下，Copilot 的反馈会包含建议的修改，你只需点击几下即可应用这些修改。

For a full introduction to GitHub Copilot code review, see [About GitHub Copilot code review](/en/copilot/concepts/code-review).

有关 GitHub Copilot code review 的完整介绍，请参阅 [About GitHub Copilot code review](/en/copilot/concepts/code-review)。

Copilot code review is also available for organization members without a Copilot license, when enabled by an enterprise administrator or organization owner. See [Copilot code review for organization members without a Copilot license](/en/copilot/concepts/agents/code-review#copilot-code-review-for-organization-members-without-a-copilot-license).

当企业管理员或组织所有者启用后，没有 Copilot 许可证的组织成员也可以使用 Copilot code review。请参阅 [Copilot code review for organization members without a Copilot license](/en/copilot/concepts/agents/code-review#copilot-code-review-for-organization-members-without-a-copilot-license)。

## Using Copilot code review in the GitHub web interface

These instructions explain how to use Copilot code review in the GitHub website. To see instructions for other popular coding environments, click the appropriate tab at the top of the page.

以下说明介绍了如何在 GitHub 网站上使用 Copilot code review。要查看其他常用编码环境中的说明，请点击页面顶部的相应标签页。

1. On GitHub.com, create a pull request (拉取请求) or navigate to an existing pull request.

1. 在 GitHub.com 上，创建一个 pull request（拉取请求）或导航到现有的 pull request。

2. Under "Reviewers" in the right sidebar, next to **Copilot**, click **Request**.

   (Screenshot showing the "Request" button next to Copilot under the "Reviewers" section.)

2. 在右侧边栏的 "Reviewers" 下，**Copilot** 旁边，点击 **Request**。

   （截图展示了 "Reviewers" 下 Copilot 旁边的 "Request" 按钮。）

3. Wait for Copilot to review your pull request. This usually takes less than 30 seconds.

3. 等待 Copilot 审查你的 pull request。通常不到 30 秒即可完成。

4. Scroll down and read through Copilot's comments.

   (Screenshot showing a code review comment left by Copilot.)

4. 向下滚动并阅读 Copilot 的评论。

   （截图展示了 Copilot 留下的代码审查评论。）

   Copilot always leaves a "Comment" review, not an "Approve" review or a "Request changes" review. This means that Copilot's reviews do not count toward required approvals for the pull request, and Copilot's reviews will not block merging changes. For more details, see [Approving a pull request with required reviews](/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/approving-a-pull-request-with-required-reviews).

   Copilot 始终留下的是 "Comment"（评论）类型的审查，而不是 "Approve"（批准）或 "Request changes"（请求修改）类型的审查。这意味着 Copilot 的审查不计入 pull request 所需的批准数量，Copilot 的审查也不会阻止合并更改。更多详情，请参阅 [Approving a pull request with required reviews](/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/approving-a-pull-request-with-required-reviews)。

5. Copilot's review comments behave like review comments from humans. You can add reactions to them, comment on them, resolve them and hide them.

5. Copilot 的审查评论与人工审查评论的行为一致。你可以对其进行表情回应、添加评论、标记为已解决以及隐藏。

   Any comments you add to Copilot's review comments will be visible to humans, but they won't be visible to Copilot, and Copilot won't reply.

   你在 Copilot 审查评论下添加的任何评论对人类可见，但 Copilot 无法看到这些评论，也不会进行回复。

You can also request a review from Copilot through the GitHub REST API by requesting `copilot-pull-request-reviewer[bot]` as a reviewer. For more information, see [REST API endpoints for review requests](/en/rest/pulls/review-requests#request-reviewers-for-a-pull-request).

你也可以通过 GitHub REST API 请求 Copilot 进行审查，方法是将 `copilot-pull-request-reviewer[bot]` 指定为审查者。更多信息，请参阅 [REST API endpoints for review requests](/en/rest/pulls/review-requests#request-reviewers-for-a-pull-request)。

## Enabling automatic reviews

By default, you manually request a review from Copilot on each pull request, in the same way you would request a review from a human. However, you can set up Copilot to automatically review all pull requests. See [Configuring automatic code review by GitHub Copilot](/en/copilot/how-tos/agents/copilot-code-review/automatic-code-review).

默认情况下，你需要在每个 pull request 上手动向 Copilot 请求审查，就像向人类请求审查一样。不过，你可以设置 Copilot 自动审查所有 pull request。请参阅 [Configuring automatic code review by GitHub Copilot](/en/copilot/how-tos/agents/copilot-code-review/automatic-code-review)。

## Working with suggested changes provided by Copilot

Where possible, Copilot's feedback includes suggested changes which you can apply with a couple of clicks.

在可能的情况下，Copilot 的反馈会包含建议的修改，你只需点击几下即可应用。

If you're happy with the changes, you can accept a single suggestion from Copilot and commit it, or accept a group of suggestions together in a single commit. For more information, see [Incorporating feedback in your pull request](/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/incorporating-feedback-in-your-pull-request).

如果你对修改满意，可以接受 Copilot 的单个建议并提交，也可以将一组建议一起接受并在一次提交中完成。更多信息，请参阅 [Incorporating feedback in your pull request](/en/pull-requests/collaborating-with-pull-requests/reviewing-changes-in-pull-requests/incorporating-feedback-in-your-pull-request)。

You can also invoke Copilot cloud agent (云端智能体) to implement suggested changes. To do this, you must:

你还可以调用 Copilot cloud agent（云端智能体）来实现建议的修改。为此，你需要：

* Enable GitHub Copilot code review and Copilot cloud agent.
* On review comments from GitHub Copilot code review, click **Fix with Copilot**. This creates a draft comment on the pull request, where you can instruct Copilot to address specific feedback. You can then select whether Copilot will create a new pull request against your branch or a commit to the same pull request with the suggestions applied.

* 启用 GitHub Copilot code review 和 Copilot cloud agent。
* 在 GitHub Copilot code review 的审查评论上，点击 **Fix with Copilot**。这会在 pull request 上创建一条草稿评论，你可以在其中指示 Copilot 处理特定反馈。然后你可以选择是让 Copilot 针对你的分支创建一个新的 pull request，还是将建议的修改作为提交应用到同一个 pull request 中。

## Requesting a re-review from Copilot

When you push changes to a pull request that Copilot has reviewed, it won't automatically re-review your changes unless you've configured it to review new pushes after enabling automatic reviews.

当你向已被 Copilot 审查过的 pull request 推送更改时，除非你在启用自动审查后配置了审查新推送，否则 Copilot 不会自动重新审查你的更改。

To manually request a re-review from Copilot, click the re-request review button next to Copilot's name in the **Reviewers** menu. For more information, see [Requesting a pull request review](/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review).

要手动请求 Copilot 重新审查，请点击 **Reviewers** 菜单中 Copilot 名称旁边的重新请求审查按钮。更多信息，请参阅 [Requesting a pull request review](/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/requesting-a-pull-request-review)。

To automatically request re-reviews from Copilot on every push, enable automatic code review for the repository and select **Review new pushes** in the ruleset settings. For more information, see [Configuring automatic code review by GitHub Copilot](/en/copilot/copilot-on-github/set-up-copilot/configure-automatic-review#configuring-automatic-code-review-for-repositories-in-an-organization).

要在每次推送时自动请求 Copilot 重新审查，请为仓库启用自动代码审查，并在规则集设置中选择 **Review new pushes**。更多信息，请参阅 [Configuring automatic code review by GitHub Copilot](/en/copilot/copilot-on-github/set-up-copilot/configure-automatic-review#configuring-automatic-code-review-for-repositories-in-an-organization)。

> [!NOTE] When re-reviewing a pull request, Copilot may repeat the same comments again, even if they have been dismissed with the "Resolve conversation" button or downvoted with the thumbs down (:-1:) button.

> [!NOTE] 重新审查 pull request 时，Copilot 可能会重复相同的评论，即使这些评论已通过 "Resolve conversation" 按钮关闭或通过踩（:-1:）按钮表示反对。

## Customizing Copilot's reviews with custom instructions

You can customize Copilot code review by adding custom instructions to your repository.

你可以通过在仓库中添加自定义指令来定制 Copilot code review。

Repository custom instructions can either be repository wide or path specific. You specify repository-wide custom instructions in a `.github/copilot-instructions.md` file in your repository. You can use this file to store information that you want Copilot to consider when reviewing code anywhere in the repository.

仓库自定义指令可以是仓库级别的，也可以是针对特定路径的。你在仓库中的 `.github/copilot-instructions.md` 文件中指定仓库级别的自定义指令。你可以使用此文件存储希望 Copilot 在审查仓库中任意位置的代码时考虑的信息。

You can also write instructions that Copilot will only use when reviewing code in files that match a specified path. You write these instructions in one or more `.github/instructions/**/*.instructions.md` files.

你还可以编写指令，使 Copilot 仅在审查匹配指定路径的文件中的代码时使用。这些指令编写在一个或多个 `.github/instructions/**/*.instructions.md` 文件中。

For more information, see [Adding repository custom instructions for GitHub Copilot](/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).

更多信息，请参阅 [Adding repository custom instructions for GitHub Copilot](/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot)。

> [!NOTE]
> When reviewing a pull request, Copilot uses the custom instructions in the base branch of the pull request. For example, if your pull request seeks to merge `my-feature-branch` into `main`, Copilot will use the custom instructions in `main`.

> [!NOTE]
> 审查 pull request 时，Copilot 使用 pull request 基础分支中的自定义指令。例如，如果你的 pull request 旨在将 `my-feature-branch` 合并到 `main`，Copilot 将使用 `main` 中的自定义指令。

### Example

This example of a `.github/copilot-instructions.md` file contains three instructions that will be applied to all Copilot code reviews in the repository.

以下 `.github/copilot-instructions.md` 文件示例包含三条指令，将应用于仓库中的所有 Copilot code review。

```text
When performing a code review, respond in Spanish.

When performing a code review, apply the checks in the `/security/security-checklist.md` file.

When performing a code review, focus on readability and avoid nested ternary operators.
```

```text
When performing a code review, respond in Spanish.

When performing a code review, apply the checks in the `/security/security-checklist.md` file.

When performing a code review, focus on readability and avoid nested ternary operators.
```

## MCP servers and agent skills

> [!NOTE]
> Support for agent skills and MCP servers with Copilot code review is in public preview and subject to change.

> [!NOTE]
> Copilot code review 对 agent skill（智能体技能）和 MCP server（MCP 服务器，Model Context Protocol，模型上下文协议）的支持处于公开预览阶段，可能会有变更。

Copilot code review can use agent skills and MCP servers configured in the repository, when they are relevant to the code being reviewed.

当与被审查的代码相关时，Copilot code review 可以使用仓库中配置的 agent skill 和 MCP server。

To make these available for Copilot code review on GitHub, configure:

要使这些功能在 GitHub 上可用于 Copilot code review，请配置：

* **Agent skills** in your repository (in `.github/skills`). If you want a skill to target review tasks, use a review-focused skill directory name such as `code-review`. For setup details, see [Adding agent skills for GitHub Copilot](/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills).
* **MCP servers** in repository Copilot settings. The GitHub MCP server and Playwright MCP server are enabled by default. For setup details, see [Configure MCP servers for your repository](/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers).

* 仓库中的 **Agent skills**（在 `.github/skills` 目录下）。如果你希望某个 skill 针对审查任务，请使用以审查为中心的 skill 目录名，例如 `code-review`。设置详情，请参阅 [Adding agent skills for GitHub Copilot](/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/add-skills)。
* 仓库 Copilot 设置中的 **MCP servers**。GitHub MCP server 和 Playwright MCP server 默认已启用。设置详情，请参阅 [Configure MCP servers for your repository](/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers)。

Copilot code review is more likely to use this context when:

在以下情况下，Copilot code review 更有可能使用这些上下文：

* Agent skills directories have review-focused names and descriptions, such as `code-review`, that indicate they are intended for pull request review.
* Your agent skills or custom instructions explicitly tell Copilot code review to use specific MCP context.
* Pull request descriptions reference items available through configured MCP servers, such as issue keys or incident IDs.

* Agent skill 目录具有以审查为中心的名称和描述（如 `code-review`），表明它们用于 pull request 审查。
* 你的 agent skill 或自定义指令明确告知 Copilot code review 使用特定的 MCP 上下文。
* Pull request 描述引用了通过已配置的 MCP server 可用的条目，例如 issue key 或 incident ID。

To verify which MCP context Copilot code review used for a specific review, open the linked review session from the pull request timeline, then check the session logs to see which MCP servers and tools were called.

要验证 Copilot code review 在某次具体审查中使用了哪些 MCP 上下文，请从 pull request 时间线中打开关联的审查会话，然后查看会话日志以了解调用了哪些 MCP server 和工具。

In repository settings, **Allow Copilot to use MCP tools when reviewing pull requests** is enabled by default. Disable this setting if you want MCP servers available only for Copilot cloud agent, and not for Copilot code review. For step-by-step instructions, see [Configure MCP servers for your repository](/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers#disabling-mcp-tools-for-code-review).

在仓库设置中，**Allow Copilot to use MCP tools when reviewing pull requests** 默认已启用。如果你希望 MCP server 仅对 Copilot cloud agent 可用，而不用于 Copilot code review，请禁用此设置。详细操作步骤，请参阅 [Configure MCP servers for your repository](/en/copilot/how-tos/copilot-on-github/customize-copilot/configure-mcp-servers#disabling-mcp-tools-for-code-review)。

## Providing feedback on Copilot's reviews

You can provide feedback on Copilot's comments directly within each comment. We use this information to improve the product and the quality of Copilot's suggestions.

你可以直接在每条评论中对 Copilot 的评论提供反馈。我们使用这些信息来改进产品和 Copilot 建议的质量。

To provide feedback on a review comment from Copilot, click the thumbs up (:+1:) or thumbs down (:-1:) button.

要对 Copilot 的审查评论提供反馈，请点击点赞（:+1:）或踩（:-1:）按钮。

(Screenshot showing a Copilot code review comment with the thumbs up and thumbs down buttons.)

（截图展示了带有点赞和踩按钮的 Copilot code review 评论。）

## Using Copilot code review in Visual Studio Code

### Reviewing a selection of code

You can request an initial review of a highlighted selection of code in Visual Studio Code.

你可以在 Visual Studio Code 中对高亮选中的代码片段请求初始审查。

1. In Visual Studio Code, select the code you want to review.
2. Right-click the selected code and choose **Generate Code** > **Review**.
3. VS Code creates review comments in the **Comments** panel and also shows them inline in the editor.

1. 在 Visual Studio Code 中，选中你想要审查的代码。
2. 右键点击选中的代码，选择 **Generate Code** > **Review**。
3. VS Code 会在 **Comments** 面板中创建审查评论，并同时在内联编辑器中显示。

### Reviewing all uncommitted changes

You can request a review of your uncommitted changes in Visual Studio Code.

你可以在 Visual Studio Code 中请求审查未提交的更改。

1. In VS Code, click the **Source Control** button in the Activity Bar.

1. 在 VS Code 中，点击活动栏中的 **Source Control** 按钮。

2. At the top of the **Source Control** view, hover over **CHANGES**, then click the **Copilot Code Review - Uncommitted Changes** button.

   (Screenshot of the "Source Control" view. The code review button is outlined in dark orange.)

2. 在 **Source Control** 视图顶部，将鼠标悬停在 **CHANGES** 上，然后点击 **Copilot Code Review - Uncommitted Changes** 按钮。

   （"Source Control" 视图截图。代码审查按钮以深橙色标出。）

3. Wait for Copilot to review your changes. This usually takes less than 30 seconds.

3. 等待 Copilot 审查你的更改。通常不到 30 秒即可完成。

4. If Copilot has any comments, they will be shown inline in your file(s), and in the **Problems** tab.

4. 如果 Copilot 有任何评论，它们将内联显示在你的文件中，以及 **Problems** 标签页中。

## Working with suggested changes provided by Copilot (VS Code)

Where possible, Copilot's feedback includes suggested changes which you can apply with a single click.

在可能的情况下，Copilot 的反馈会包含建议的修改，你只需点击一次即可应用。

(Screenshot of a comment from Copilot in Visual Studio Code with a suggested change.)

（截图展示了 Visual Studio Code 中带有建议修改的 Copilot 评论。）

If you're happy with the change, you can accept a suggestion from Copilot by clicking the **Apply and Go To Next** button. Any changes you apply will not be automatically committed.

如果你对修改满意，可以通过点击 **Apply and Go To Next** 按钮来接受 Copilot 的建议。你应用的所有更改不会自动提交。

If you don't want to apply Copilot's suggested change, click the **Discard and Go to Next** button.

如果你不想应用 Copilot 建议的修改，请点击 **Discard and Go to Next** 按钮。

## Providing feedback on Copilot's reviews (VS Code)

You can provide feedback on Copilot's comments directly within each comment. We use this information to improve the product and the quality of Copilot's suggestions.

你可以直接在每条评论中对 Copilot 的评论提供反馈。我们使用这些信息来改进产品和 Copilot 建议的质量。

To provide feedback, hover over the comment and click the thumbs up or thumbs down button.

要提供反馈，请将鼠标悬停在评论上，然后点击点赞或踩按钮。

(Screenshot of a comment from Copilot in Visual Studio Code with feedback buttons displayed. The buttons are outlined in dark orange.)

（截图展示了 Visual Studio Code 中显示反馈按钮的 Copilot 评论。按钮以深橙色标出。）

## Customizing Copilot's reviews with custom instructions (VS Code)

You can customize Copilot code review by adding custom instructions to your repository.

你可以通过在仓库中添加自定义指令来定制 Copilot code review。

Repository custom instructions can either be repository wide or path specific. You specify repository-wide custom instructions in a `.github/copilot-instructions.md` file in your repository. You can use this file to store information that you want Copilot to consider when reviewing code anywhere in the repository.

仓库自定义指令可以是仓库级别的，也可以是针对特定路径的。你在仓库中的 `.github/copilot-instructions.md` 文件中指定仓库级别的自定义指令。你可以使用此文件存储希望 Copilot 在审查仓库中任意位置的代码时考虑的信息。

You can also write instructions that Copilot will only use when reviewing code in files that match a specified path. You write these instructions in one or more `.github/instructions/**/*.instructions.md` files.

你还可以编写指令，使 Copilot 仅在审查匹配指定路径的文件中的代码时使用。这些指令编写在一个或多个 `.github/instructions/**/*.instructions.md` 文件中。

For more information, see [Adding repository custom instructions for GitHub Copilot](/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot).

更多信息，请参阅 [Adding repository custom instructions for GitHub Copilot](/en/copilot/customizing-copilot/adding-repository-custom-instructions-for-github-copilot)。

> [!NOTE]
> When reviewing a pull request, Copilot uses the custom instructions in the base branch of the pull request. For example, if your pull request seeks to merge `my-feature-branch` into `main`, Copilot will use the custom instructions in `main`.

> [!NOTE]
> 审查 pull request 时，Copilot 使用 pull request 基础分支中的自定义指令。例如，如果你的 pull request 旨在将 `my-feature-branch` 合并到 `main`，Copilot 将使用 `main` 中的自定义指令。

### Example (VS Code)

This example of a `.github/copilot-instructions.md` file contains three instructions that will be applied to all Copilot code reviews in the repository.

以下 `.github/copilot-instructions.md` 文件示例包含三条指令，将应用于仓库中的所有 Copilot code review。

```text
When performing a code review, respond in Spanish.

When performing a code review, apply the checks in the `/security/security-checklist.md` file.

When performing a code review, focus on readability and avoid nested ternary operators.
```

```text
When performing a code review, respond in Spanish.

When performing a code review, apply the checks in the `/security/security-checklist.md` file.

When performing a code review, focus on readability and avoid nested ternary operators.
```

## Using Copilot code review in Visual Studio

### Prerequisite

To use Copilot code review, you must use Visual Studio version 17.14 or later. See the [Visual Studio downloads page](https://visualstudio.microsoft.com/downloads/).

### 前提条件

要使用 Copilot code review，你必须使用 Visual Studio 17.14 或更高版本。请参阅 [Visual Studio 下载页面](https://visualstudio.microsoft.com/downloads/)。

### Using Copilot code review

These instructions explain how to use Copilot code review in Visual Studio. To see instructions for other popular coding environments, click the appropriate tab at the top of the page.

### 使用 Copilot code review

以下说明介绍了如何在 Visual Studio 中使用 Copilot code review。要查看其他常用编码环境中的说明，请点击页面顶部的相应标签页。

1. In the Git Changes window, click **Review changes with Copilot**.
   This button appears as a comment icon with a sparkle.

1. 在 Git Changes 窗口中，点击 **Review changes with Copilot**。
   此按钮显示为带有闪光效果的评论图标。

2. Copilot will begin reviewing your changes. After a few moments, a link showing the number of code review comments appears in the Git Changes window.

2. Copilot 将开始审查你的更改。片刻之后，Git Changes 窗口中会出现一个显示代码审查评论数量的链接。

3. Click the link to view and navigate the comments. If no issues are found, you'll see the message:
   Copilot did not comment on any files.

3. 点击链接查看并导航评论。如果未发现问题，你将看到以下消息：
   Copilot did not comment on any files.

4. Copilot displays comments in your code with a summary of each potential issue. You can:

   * Review and make changes based on the suggestions.
   * Dismiss a comment using the downward arrow in the top-right corner of the comment box.

4. Copilot 在代码中显示评论，并附带每个潜在问题的摘要。你可以：

   * 根据建议进行审查和修改。
   * 使用评论框右上角的向下箭头关闭评论。

5. To remove all review comments, click the X icon next to the code review link in the Git Changes window.

5. 要移除所有审查评论，请点击 Git Changes 窗口中代码审查链接旁边的 X 图标。

For more information on enabling and configuring Copilot code review in Visual Studio, see [Review local changes with Copilot Chat](https://learn.microsoft.com/en-us/visualstudio/version-control/git-make-commit?view=vs-2022#review-local-changes-with-copilot-chat) in the Visual Studio documentation.

有关在 Visual Studio 中启用和配置 Copilot code review 的更多信息，请参阅 Visual Studio 文档中的 [Review local changes with Copilot Chat](https://learn.microsoft.com/en-us/visualstudio/version-control/git-make-commit?view=vs-2022#review-local-changes-with-copilot-chat)。

## Using Copilot code review in GitHub Mobile

These instructions explain how to use Copilot code review in GitHub Mobile. To see instructions for other popular coding environments, click the appropriate tab at the top of the page.

以下说明介绍了如何在 GitHub Mobile 中使用 Copilot code review。要查看其他常用编码环境中的说明，请点击页面顶部的相应标签页。

1. In GitHub Mobile, open a pull request.
2. Scroll down to the **Reviews** section and expand it.
3. Click **Request Reviews**.
4. Add Copilot as a reviewer, then click **Done**.

1. 在 GitHub Mobile 中，打开一个 pull request。
2. 向下滚动到 **Reviews** 部分并展开。
3. 点击 **Request Reviews**。
4. 将 Copilot 添加为审查者，然后点击 **Done**。

Copilot will review the changes and provide feedback.

Copilot 将审查更改并提供反馈。

## Using Copilot code review in Xcode

### Prerequisite

To use Copilot code review in Xcode, you must use version 0.41.0 or later of the GitHub Copilot Chat extension. Download the latest release from the [`github/CopilotForXcode` repository](https://github.com/github/CopilotForXcode/releases/latest).

### 前提条件

要在 Xcode 中使用 Copilot code review，你必须使用 0.41.0 或更高版本的 GitHub Copilot Chat 扩展。请从 [`github/CopilotForXcode` 仓库](https://github.com/github/CopilotForXcode/releases/latest)下载最新版本。

### Using Copilot code review

These instructions explain how to use Copilot code review in Xcode. To see instructions for other popular coding environments, click the appropriate tab at the top of the page.

### 使用 Copilot code review

以下说明介绍了如何在 Xcode 中使用 Copilot code review。要查看其他常用编码环境中的说明，请点击页面顶部的相应标签页。

1. In Xcode, make some changes to one or more files.

1. 在 Xcode 中，对一个或多个文件进行一些更改。

2. Open the Copilot chat window by clicking **Editor** in the menu bar, clicking **GitHub Copilot** then **Open Chat**.

2. 通过点击菜单栏中的 **Editor**，然后点击 **GitHub Copilot**，再点击 **Open Chat** 来打开 Copilot 聊天窗口。

3. Near the bottom right of the prompt (提示词) box in the Copilot chat window, click the **Code Review** button (a speech bubble icon).

   (Screenshot of the Copilot chat window in Xcode, with the 'Code Review' button outlined in dark orange.)

3. 在 Copilot 聊天窗口中 prompt（提示词）输入框的右下角附近，点击 **Code Review** 按钮（一个对话气泡图标）。

   （Xcode 中 Copilot 聊天窗口截图，"Code Review" 按钮以深橙色标出。）

4. Click either **Review Staged Changes** or **Review Unstaged Changes**.

4. 点击 **Review Staged Changes** 或 **Review Unstaged Changes**。

5. A list of files containing changes is displayed in the chat window. Click the check boxes to deselect any files you don't want Copilot to review.

5. 聊天窗口中会显示包含更改的文件列表。点击复选框以取消选择你不想让 Copilot 审查的文件。

6. Click **Continue** to start the review process.

6. 点击 **Continue** 开始审查流程。

7. If Copilot finds things to comment on, it displays a **Reviewed Changes** list in the chat window, listing the files it has commented on. Click a file in this list to see the comments.

   Each comment is shown in a popup, overlaid over the editor.

   (Screenshot of a Copilot code review review comment.)

7. 如果 Copilot 发现需要评论的内容，它会在聊天窗口中显示一个 **Reviewed Changes** 列表，列出它已评论的文件。点击列表中的某个文件即可查看评论。

   每条评论显示在叠加在编辑器上方的弹出窗口中。

   （Copilot code review 审查评论截图。）

8. If there is more than one comment in the file, use the up and down arrows, at the top right of the popup, to navigate between comments.

8. 如果文件中有多条评论，请使用弹出窗口右上角的上下箭头在评论之间导航。

9. Copilot may suggest replacement code. You can apply the suggested change by clicking **Accept** or reject it by clicking **Dismiss**.

9. Copilot 可能会建议替换代码。你可以通过点击 **Accept** 来应用建议的修改，或通过点击 **Dismiss** 来拒绝。

10. Click another file in the **Reviewed Changes** list in the chat window, to see the review comments for another file.

10. 点击聊天窗口中 **Reviewed Changes** 列表中的另一个文件，以查看另一个文件的审查评论。

## Using Copilot code review in JetBrains IDEs

### Prerequisites

* **Access to Copilot**. See [What is GitHub Copilot?](/en/copilot/about-github-copilot/what-is-github-copilot#getting-access-to-copilot).

### 前提条件

* **Copilot 的访问权限**。请参阅 [What is GitHub Copilot?](/en/copilot/about-github-copilot/what-is-github-copilot#getting-access-to-copilot)。

* **Compatible JetBrains IDE**. To use GitHub Copilot in JetBrains, you must have a compatible JetBrains IDE installed. GitHub Copilot is compatible with the following IDEs:

  * IntelliJ IDEA (Ultimate, Community, Educational)
  * Android Studio
  * CLion
  * Code With Me Guest
  * DataGrip
  * DataSpell
  * GoLand
  * JetBrains Client
  * MPS
  * PhpStorm
  * PyCharm (Professional, Community, Educational)
  * Rider
  * RubyMine
  * RustRover
  * WebStorm

  See the [JetBrains IDEs](https://www.jetbrains.com/products/?ref_product=copilot&ref_type=engagement&ref_style=button) tool finder to download.

* **兼容的 JetBrains IDE**。要在 JetBrains 中使用 GitHub Copilot，你必须安装兼容的 JetBrains IDE。GitHub Copilot 兼容以下 IDE：

  * IntelliJ IDEA (Ultimate, Community, Educational)
  * Android Studio
  * CLion
  * Code With Me Guest
  * DataGrip
  * DataSpell
  * GoLand
  * JetBrains Client
  * MPS
  * PhpStorm
  * PyCharm (Professional, Community, Educational)
  * Rider
  * RubyMine
  * RustRover
  * WebStorm

  请访问 [JetBrains IDEs](https://www.jetbrains.com/products/?ref_product=copilot&ref_type=engagement&ref_style=button) 工具查找器进行下载。

* **Latest version of the GitHub Copilot extension**. See the [GitHub Copilot plugin](https://plugins.jetbrains.com/plugin/17718-github-copilot?ref_product=copilot&ref_type=engagement&ref_style=text) in the JetBrains Marketplace. For installation instructions, see [Installing the GitHub Copilot extension in your environment](/en/copilot/how-tos/set-up/install-copilot-extension?tool=jetbrains).

* **最新版本的 GitHub Copilot 扩展**。请参阅 JetBrains Marketplace 中的 [GitHub Copilot plugin](https://plugins.jetbrains.com/plugin/17718-github-copilot?ref_product=copilot&ref_type=engagement&ref_style=text)。安装说明，请参阅 [Installing the GitHub Copilot extension in your environment](/en/copilot/how-tos/set-up/install-copilot-extension?tool=jetbrains)。

* **Sign in to GitHub in your JetBrains IDE**. For authentication instructions, see [Installing the GitHub Copilot extension in your environment](/en/copilot/how-tos/set-up/install-copilot-extension?tool=jetbrains#installing-the-github-copilot-plugin-in-your-jetbrains-ide).

* **在 JetBrains IDE 中登录 GitHub**。认证说明，请参阅 [Installing the GitHub Copilot extension in your environment](/en/copilot/how-tos/set-up/install-copilot-extension?tool=jetbrains#installing-the-github-copilot-plugin-in-your-jetbrains-ide)。

### Using Copilot code review

These instructions explain how to use Copilot code review in JetBrains IDEs. To see instructions for other popular coding environments, click the appropriate tab at the top of the page.

### 使用 Copilot code review

以下说明介绍了如何在 JetBrains IDE 中使用 Copilot code review。要查看其他常用编码环境中的说明，请点击页面顶部的相应标签页。

1. In a JetBrains IDE, make some changes to one or more files.
2. Open the "Commit" tool window on the left-hand side.
3. Above the commit message input field, click **Copilot: Review Code Changes**. This button appears as a magnifying glass icon with a sparkle.
4. Copilot will begin reviewing your changes.
5. Copilot displays comments in your code with a summary of each potential issue. You can:

   * Review and make changes based on the suggestions.
   * Dismiss a comment by clicking **Discard**.
6. If there is more than one comment, use the up and down arrows, at the top right of the popup, to navigate between comments.

1. 在 JetBrains IDE 中，对一个或多个文件进行一些更改。
2. 打开左侧的 "Commit" 工具窗口。
3. 在提交消息输入框上方，点击 **Copilot: Review Code Changes**。此按钮显示为带有闪光效果的放大镜图标。
4. Copilot 将开始审查你的更改。
5. Copilot 在代码中显示评论，并附带每个潜在问题的摘要。你可以：

   * 根据建议进行审查和修改。
   * 点击 **Discard** 关闭评论。
6. 如果有多条评论，请使用弹出窗口右上角的上下箭头在评论之间导航。

## Using Copilot code review with the GitHub CLI

### Prerequisites

* **Access to Copilot**. See [What is GitHub Copilot?](/en/copilot/about-github-copilot/what-is-github-copilot#getting-access-to-copilot).
* **GitHub CLI**. You must have the GitHub CLI installed and authenticated. See [GitHub CLI quickstart](/en/github-cli/github-cli/quickstart).

### 前提条件

* **Copilot 的访问权限**。请参阅 [What is GitHub Copilot?](/en/copilot/about-github-copilot/what-is-github-copilot#getting-access-to-copilot)。
* **GitHub CLI**。你必须已安装并认证 GitHub CLI。请参阅 [GitHub CLI 快速入门](/en/github-cli/github-cli/quickstart)。

### Using Copilot code review

These instructions explain how to use Copilot code review with the GitHub CLI. To see instructions for other popular coding environments, click the appropriate tab at the top of the page.

### 使用 Copilot code review

以下说明介绍了如何使用 GitHub CLI 进行 Copilot code review。要查看其他常用编码环境中的说明，请点击页面顶部的相应标签页。

### Requesting a review when creating a pull request

You can request a review from Copilot when creating a new pull request using `gh pr create`:

### 创建 pull request 时请求审查

你可以在使用 `gh pr create` 创建新 pull request 时请求 Copilot 审查：

```shell copy
gh pr create --reviewer @copilot
```

```shell copy
gh pr create --reviewer @copilot
```

You can also select Copilot interactively from the searchable reviewer prompt during `gh pr create`.

你也可以在 `gh pr create` 过程中从可搜索的审查者提示中交互式地选择 Copilot。

```text
? Reviewers  [Use arrows to move, space to select, <right> to all, <left> to none, type to filter]
  [ ]  Search (7472 more)
  [x]  monalisa (Mona Lisa)
> [x]  Copilot (AI)
```

```text
? Reviewers  [Use arrows to move, space to select, <right> to all, <left> to none, type to filter]
  [ ]  Search (7472 more)
  [x]  monalisa (Mona Lisa)
> [x]  Copilot (AI)
```

### Requesting a review on an existing pull request

To request a review from Copilot on an existing pull request, use `gh pr edit`. If you are not on the pull request's branch, specify the pull request number:

### 对现有 pull request 请求审查

要对现有 pull request 请求 Copilot 审查，请使用 `gh pr edit`。如果你不在该 pull request 的分支上，请指定 pull request 编号：

```shell copy
gh pr edit PR-NUMBER --add-reviewer @copilot
```

```shell copy
gh pr edit PR-NUMBER --add-reviewer @copilot
```

Replace `PR-NUMBER` with the number of the pull request you want reviewed. If you have the pull request's branch checked out, you can omit the number.

将 `PR-NUMBER` 替换为你想要审查的 pull request 编号。如果你已检出该 pull request 的分支，则可以省略编号。

---

## Responsible AI（负责任的 AI）

原文链接：https://responsibleaidatabase.com

### Module Overview / 模块概述

# Responsible AI with GitHub Copilot

- Module
- 5 Units

Beginner

AI Engineer

Developer

DevOps Engineer

Student

GitHub

# Responsible AI with GitHub Copilot（GitHub Copilot 的负责任 AI 使用）

- 模块（Module）
- 5 个单元（Units）

初级

AI 工程师（AI Engineer）

开发者（Developer）

DevOps 工程师（DevOps Engineer）

学生

GitHub

This module explores the responsible use of AI in the context of GitHub Copilot, a generative AI tool for developers. It will equip you with the knowledge and skills to leverage Copilot effectively while mitigating potential ethical and operational risks associated with AI usage.

本模块探讨在 GitHub Copilot——一款面向开发者的生成式 AI 工具——这一语境下如何负责任地使用 AI。它将帮助您掌握相关知识与技能，以便在有效利用 Copilot 的同时，缓解与 AI 使用相关的潜在伦理与运营风险。

### Learning objectives / 学习目标

By the end of this module, students will be able to:

- Understand and apply the principles of Responsible AI usage.
- Identify limitations and mitigate risks associated with AI.
- Learn best practices for ensuring AI-generated code aligns with ethical standards and project-specific requirements.
- Recognize the importance of transparency and accountability in AI systems to build trust and maintain user confidence.

学完本模块后，学员将能够：

- 理解并应用负责任 AI（Responsible AI）使用的原则。
- 识别与 AI 相关的局限性，并缓解相关风险。
- 学习最佳实践，确保 AI 生成的代码符合伦理标准与项目特定需求。
- 认识到 AI 系统中透明性（transparency）与问责性（accountability）的重要性，以建立信任并维持用户信心。

### Prerequisites / 前置条件

A basic understanding of generative AI.

对生成式 AI（generative AI）有基本的了解。

### Module units / 模块单元

- [Introduction](1-introduction)min
- [Mitigate AI risks](2-manage-ai-risks)min
- [Microsoft and GitHub's six principles of responsible AI](3-six-principles-of-responsible-ai)min
- [Module assessment](4-knowledge-check)min
- [Summary](5-summary)min

- [简介（Introduction）](1-introduction) 分钟
- [缓解 AI 风险（Mitigate AI risks）](2-manage-ai-risks) 分钟
- [Microsoft 与 GitHub 的负责任 AI 六大原则（Microsoft and GitHub's six principles of responsible AI）](3-six-principles-of-responsible-ai) 分钟
- [模块评估（Module assessment）](4-knowledge-check) 分钟
- [总结（Summary）](5-summary) 分钟

### Module assessment / 模块评估

Take the module assessment

Module Assessment Results

Assess your understanding of this module. Sign in and answer all questions correctly to earn a pass designation on your profile.

参加模块评估

（此处为模块评估结果展示组件，需登录后作答）

评估您对本模块的理解。请登录并正确回答所有问题，即可在您的个人资料中获得通过标识。

---
