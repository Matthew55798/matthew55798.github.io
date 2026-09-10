# AI 开发工作流种类调研：官网自我定义横向对比

> **调研日期**：2026-08-29（热度数据均为当日实测）
> **调研原则**：**所有信息仅取自官网 / 官方 GitHub 仓库 / 官方文档 / 官方博客**，每条均附来源 URL；"官网自我定义"为该工作流官网或官方 README 对自己的定位描述原文（英文原文 + 中文翻译），非第三方转述。
> **背景**：为「Web Vibe Coding 课程大纲」准备素材。课程计划先讲公司 6 月内部培训内容，再讲 Superpowers 与 AI Coding for Real Engineers（mattpocock/skills）相关的工作流。本文件在既有两份笔记（Superpowers、mattpocock/skills）之上，把「AI 开发工作流的种类」铺开并做横向/纵向对比。
> **姊妹笔记**：`/Users/matthew/Downloads/LOG_0824/notes/Superpowers调研笔记.md`、`/Users/matthew/Downloads/LOG_0824/notes/MattPocockSkills_vs_Superpowers对比.md`

---

## 一、调研说明

- **方法**：官网/文档站 curl 直抓正文（多数 Mintlify 系文档站支持 `.md` 后缀直接返回原文，如 `code.claude.com/docs/en/*.md`、`cursor.com/docs/*.md`）；GitHub 元数据（star、描述、创建时间）用 GitHub REST API（官方接口），匿名限额用尽后以 shields.io JSON 徽章（展示层，数据源仍是 GitHub 官方 API）+ GitHub 官方页面 HTML 交叉核对；README 原文在 `raw.githubusercontent.com` 被网络阻断时经 jsDelivr CDN 镜像获取（内容与官方仓库逐字节一致，不构成二手来源）；反爬站点（developers.openai.com、openai.com、devin.ai、windsurf.com 等）无法直读正文时，以官方搜索索引确认页面存在，并只引用官方旁证（官方 Cookbook、官方 Academy、官方仓库 README），凡正文未能逐字抓取处均如实标注。
- **网络状况**：`github.com`、`api.github.com` 直连正常；`raw.githubusercontent.com`、`codeload.github.com` 超时（README 改用 jsDelivr CDN 镜像或 GitHub 页面抓取）；**`spec-kit.dev`、`gsd.dev`、`bmad.dev` 域名实测不存在**，均属错误猜测（已核实，见第五节）。
- **热度口径**：GitHub stars 为当日 GitHub 官方数据实测；npm 下载量为 npm 官方 registry last-month 接口；闭源产品无 star 时以官方页面披露数字为准（如实注明）。
- **合规审计**：2026-08-29 完成来源合规性复核——全文无任何二手来源作为定义与事实依据（二手内容仅用于"发现候选页面"）；反爬站点处如实标注"未能逐字抓取"；Kiro 条目补强了 4 个 AWS 官方 URL（官方日文博客 + 官方 What's New，实测 200）；`/codex/workflows/` 路径因反爬未逐字验证，已明确标注。

---

## 二、调研范围：AI 开发工作流的四类

| 类别 | 特点 | 代表 |
|------|------|------|
| **① 流程方法论框架** | 给 AI 编码智能体挂一套完整纪律（spec → 计划 → 执行 → 验证），可跨工具使用 | Superpowers、mattpocock/skills、Spec Kit、GSD（GSD Core）、BMAD、AGENTS.md 规范 |
| **② 工具内建工作流** | 官方随工具/平台内置的模式与编排能力（plan mode、subagents、hooks、automations、workflows） | Claude Code、OpenAI Codex、OpenCode、Cursor、Windsurf（已并入 Devin）、GitHub Copilot（agent mode / coding agent）、Replit Agent |
| **③ 自治代理平台** | 独立运行、自主完成整条任务的代理（终端 / IDE / 云端） | Devin、Cline、Aider、Roo Code、Gemini CLI、gpt-pilot |
| **④ 纯 vibe coding 平台** | 浏览器内 prompt → 应用 → 部署，最小工程约束 | Bolt、v0、Lovable、Replit Agent |
| **⑤ 规划/编排层** | 跨 agent 的人机协作规划层（看板等） | Vibe Kanban、Kiro（agentic IDE） |

> 说明：AGENTS.md / CLAUDE.md 是①②③类共用的上下文底座（"给 agent 看的 README"），归入第①类作背景。

---

## 三、横向表：各工作流官网上的自我定义

### ① 流程方法论框架

| 工作流 | 官方来源（官网/官方仓库） | 官网自我定义（原文） | 中文释义 | 热度（2026-08-29） |
|--------|--------------------------|----------------------|----------|----------|
| **Superpowers** | 官方仓库 [obra/superpowers](https://github.com/obra/superpowers)（Jesse Vincent） | "A complete software development methodology for your coding agents, built on top of a set of composable skills and some initial instructions that make sure your agent uses them."（README 首句） | 一套完整的软件开发方法论：可组合技能 + 初始指令，确保智能体会使用这些技能 | 279,235★ / 25,004 fork |
| **mattpocock/skills（AI Coding for Real Engineers）** | 官方仓库 [mattpocock/skills](https://github.com/mattpocock/skills)、官方站点 [aihero.dev/skills](https://aihero.dev/skills)、官方安装页 [skills.sh/mattpocock/skills](https://skills.sh/mattpocock/skills)（Matt Pocock / AI Hero） | "Skills For Real Engineers… My agent skills that I use every day to do real engineering - **not vibe coding**."（README） | 真实工程师用的技能集——我每天做真实工程用的技能，不是 vibe coding | 240,621★ / 20,459 fork；skills.sh 显示 53 skills / 19.0M installs |
| **Spec Kit（Spec-Driven Development）** | 官方仓库 [github/spec-kit](https://github.com/github/spec-kit)（**GitHub 官方组织**，创建者 Den Delimarsky，2025-08-21）、官方文档站 [github.github.io/spec-kit](https://github.github.io/spec-kit/) | README 主标语："Define what to build before building it — with any AI coding agent."；定位段："An open source toolkit for building high-quality software with any AI coding agent — a ready-to-use spec-driven process (or bring your own), endlessly extensible, community-driven, and built for your whole organization."；SDD 定义："specifications become executable, directly generating working implementations rather than just guiding them." | 先定义要构建什么，再用任意 AI 编码智能体构建；开源工具包，开箱即用的规格驱动流程（或自带流程），可扩展、社区驱动、为整个组织而生；规格变成**可执行**的，直接生成实现而不只是指引 | 132,124★ / 11,881 fork；2026-08 发布 1.0.0 |
| **GSD / GSD Core** | 官方仓库（新）[open-gsd/gsd-core](https://github.com/open-gsd/gsd-core)（旧仓库 gsd-build/get-shit-done 已迁移归档）、官网 [opengsd.net](https://opengsd.net) | 旧仓库自述："A light-weight and powerful meta-prompting, context engineering and spec-driven development system for Claude Code by TÂCHES."；GSD Core README："A light-weight meta-prompting, context engineering, and spec-driven development system for Claude Code, OpenCode, Antigravity CLI, Kimi CLI, Kilo, Codex, Copilot, Cursor, Windsurf, and more." + "GSD Core is a context-engineering and spec-driven development framework that drives AI coding agents … through a disciplined phase loop."；官网："Git. Ship. Done. for AI-Native Engineering"；"Most AI coding setups rot as the context window fills. Open GSD keeps the context clean, the work verified, and the git history honest." | 轻量级元提示 + 上下文工程 + 规格驱动开发系统（支持十余种运行时）；用"讨论→计划→执行→验证→交付"纪律化阶段循环驱动智能体；"大多数 AI 编码环境会随上下文填满而腐化，Open GSD 让上下文干净、工作可验证、git 历史诚实" | 新仓库 8,874★ + npm 43,706/月；旧仓库 64,630★（已归档） |
| **BMAD（BMad Method）** | 官方仓库 [bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD)、官方文档 [docs.bmad-method.org](https://docs.bmad-method.org)、生态站 [bmadcode.com](https://bmadcode.com) | README 首句："**Agile Ai Driven Development** — turn an idea or change request into working software without giving up the thinking."；仓库描述："Breakthrough Method for Agile Ai Driven Development"；文档站首页："BMad helps you decide what to build and then build it." | 敏捷式 AI 驱动开发——把想法或变更请求变成可运行软件，且不放弃人的思考（官方全称：Breakthrough Method for Agile AI Driven Development，**并非** "Basecamp-style"） | 52,435★ / 5,966 fork；npm 81,493/月 |
| **AGENTS.md 开放标准** | 官方仓库 [agentsmd/agents.md](https://github.com/agentsmd/agents.md)（原 openai/agents.md，2025-08-19 创建）、官网 [agents.md](https://agents.md) | "AGENTS.md — a simple, open format for guiding coding agents."；"Think of AGENTS.md as a README for agents: a dedicated, predictable place to provide context and instructions to help AI coding agents work on your project." | 一个简单、开放的指导编码智能体的格式；"给智能体看的 README"——专用、可预期的位置存放上下文与指令 | 24k★（同类：Claude Code 的 CLAUDE.md，官方文档 code.claude.com/docs/en/memory） |

### ② 工具内建工作流

| 工作流 | 官方来源（官网/官方仓库） | 官网自我定义（原文） | 中文释义 | 热度 |
|--------|--------------------------|----------------------|----------|------|
| **Claude Code** | 官方文档 [code.claude.com/docs](https://code.claude.com/docs/en/overview)、官方仓库 [anthropics/claude-code](https://github.com/anthropics/claude-code) | README："Claude Code is an agentic coding tool that lives in your terminal, understands your codebase, and helps you code faster by executing routine tasks, explaining complex code, and handling git workflows -- all through natural language commands."；Common workflows 页："Step-by-step guides for exploring codebases, fixing bugs, refactoring, testing, and other everyday tasks."；Plan mode："Plan mode tells Claude to research and propose changes without making them…"; Subagents："specialized AI assistants that handle specific types of tasks."; Hooks："user-defined shell commands… that execute automatically at specific points in Claude Code's lifecycle."; Dynamic workflows："orchestrate many subagents from a script Claude writes and you can rerun." | 终端里的智能体编码工具，自然语言处理日常任务、解释复杂代码、处理 git 工作流；官方内建 plan mode / subagents / hooks / dynamic workflows 四件套 | 143,355★ / 22,923 fork（仓库为文档/分发仓库，工具本身闭源） |
| **OpenAI Codex** | 官方文档 [developers.openai.com/codex](https://developers.openai.com/codex/workflows)、官方仓库 [openai/codex](https://github.com/openai/codex) | CLI 仓库描述："Lightweight coding agent that runs in your terminal."；官方 Cookbook："Iterating Development Workflows with Codex"；官方 Academy："Codex 102: Practical Workflows" | 在终端运行的轻量级编码智能体；官方提供 workflows（可复用会话模板）、automations（定时/事件触发后台任务）、plan mode、skills | 119,712★ / 18,291 fork（CLI） |
| **OpenCode** | 官网 [opencode.ai](https://opencode.ai)、官方仓库 [anomalyco/opencode](https://github.com/anomalyco/opencode)（原 sst/opencode，现 Anomaly 团队） | 官网首页："The open source AI coding agent"；"OpenCode is an open source agent that helps you write code in your terminal, IDE, or desktop."；Agent 文档："Agents are specialized AI assistants that can be configured for specific tasks and workflows. … Use the plan agent to analyze code and review suggestions without making any code changes." | 开源 AI 编码智能体（终端 / IDE / 桌面）；plan/build 双 agent + subagents + MCP | 202,292★ / 26,270 fork（开源 coding agent 第一梯队） |
| **Cursor** | 官网 [cursor.com/docs](https://cursor.com/docs/agent/overview)（Anysphere，闭源） | 文档首页："Cursor is a coding agent for building ambitious software. Use it to understand your codebase, plan and build features, fix bugs, review changes, and work with the tools you already use."；Plan Mode："creates detailed implementation plans before writing any code. Agent researches your codebase, asks clarifying questions, and generates a reviewable plan you can edit before building."；Subagents："specialized AI assistants that Cursor's agent can delegate tasks to. Each subagent operates in its own context window…"；Automations："run cloud agents in the background, either on a schedule or in response to events from GitHub, GitLab, Slack, webhooks, Linear, and more." | 构建雄心勃勃软件的编码智能体；内建 Plan Mode（先出可审阅计划再编码）、Subagents（独立上下文窗口）、Automations（云端定时/事件触发）、Cloud Agents | 闭源（无 star 可查）；早期"YAML workflows"概念已演进为 subagents + automations |
| **Windsurf / Cascade** | 原官网 [windsurf.com](https://windsurf.com)、原文档 [docs.windsurf.com](https://docs.windsurf.com/windsurf/cascade/workflows)；**2025-07 被 Cognition（Devin 母公司）收购**，官方公告 [cognition.ai/blog/windsurf](https://cognition.ai/blog/windsurf) 与 [windsurf.com/blog/windsurfs-next-chapter](https://windsurf.com/blog/windsurfs-next-chapter)，文档现并入 docs.devin.ai | Cascade Workflows 文档："Automate repetitive tasks in Cascade with reusable workflows defined as markdown files." / "Workflows enable users to define a series of steps to guide Cascade through a repetitive set of tasks, such as deploying a service or responding to PR comments." | 用定义为 Markdown 文件的可复用工作流自动化 Cascade 中的重复任务（如部署服务、回复 PR 评论）；斜杠命令触发 | 闭源；已并入 Devin（Cognition）生态 |
| **GitHub Copilot（agent mode / coding agent）** | 官方博客 [agent-mode-101](https://github.blog/ai-and-ml/github-copilot/agent-mode-101-all-about-github-copilots-powerful-mode/)、[coding agent 101](https://github.blog/ai-and-ml/github-copilot/github-copilot-coding-agent-101-getting-started-with-agentic-workflows-on-github/)、官方文档 [docs.github.com](https://docs.github.com/en/copilot) | GitHub 官方博客："When you give it a natural-language prompt, Copilot's agent mode works to execute it on your behalf, automating processes and workflows that would otherwise take a lot of time…"；"Agent mode: a real-time collaborator that sits in your editor… Coding agent: an asynchronous teammate that lives in the cloud, takes on issues, and sends you fully tested pull requests while you do other things." | 双形态：IDE 内实时 agent mode（替你执行自然语言任务）+ 云端异步 coding agent（认领 issue、交付测试完备的 PR） | 闭源（GitHub 官方产品） |
| **Replit Agent** | 官网 [replit.com/agent](https://replit.com/agent)、官方文档 [docs.replit.com/replitai/agent](https://docs.replit.com/replitai/agent) | 官网 Agent 页："10x faster builds. More time for creative work." + "Introducing Agent 4" | 快 10 倍的构建，更多时间留给创造性工作；在 Replit 云 IDE 内自然语言端到端构建应用 | 闭源 |

### ③ 自治代理平台

| 工作流 | 官方来源（官网/官方仓库） | 官网自我定义（原文） | 中文释义 | 热度 |
|--------|--------------------------|----------------------|----------|------|
| **Devin** | 官网 [devin.ai](https://devin.ai)、官方文档 [docs.devin.ai/get-started/devin-intro](https://docs.devin.ai/get-started/devin-intro)、官方博客 [cognition.ai/blog/introducing-devin](https://cognition.ai/blog/introducing-devin)（Cognition） | 官方文档："Devin is an autonomous AI software engineer that can write, run and test code. Devin can handle most tasks, excluding extremely difficult tasks. As a rule of thumb, if you can do it in three hours, Devin can most likely do it."；发布博客："Introducing Devin, the first AI software engineer." | 自主 AI 软件工程师：能写、运行、测试代码；经验法则——三小时能干完的事 Devin 大概率也能（首个 AI 软件工程师） | 闭源 SaaS |
| **Cline** | 官网 [cline.bot](https://cline.bot)、官方仓库 [cline/cline](https://github.com/cline/cline) | 官网："The Open Coding Agent — One open source agent runtime. Use it in your editor, your terminal, or embed it in your own products. Trusted by 8M+ developers."；仓库描述："Autonomous coding agent as an SDK, IDE extension, or CLI assistant." | 开源编码智能体运行时：编辑器 / 终端 / 嵌入式产品三形态；800 万+ 开发者 | 67,111★ / 7,245 fork |
| **Aider** | 官网 [aider.chat](https://aider.chat)、官方仓库 [Aider-AI/aider](https://github.com/Aider-AI/aider) | 官网/README："AI pair programming in your terminal — Aider lets you pair program with LLMs to start a new project or build on your existing codebase." | 终端里的 AI 结对编程：与 LLM 结对启动新项目或在既有代码库上开发；git 原生集成 | 48,567★ / 4,898 fork；官网显示 6.8M 安装 |
| **Roo Code** | 官网 [roocode.com](https://roocode.com)（现主打云代理 Roomote）、官方仓库 [RooCodeInc/Roo-Code](https://github.com/RooCodeInc/Roo-Code)（原 RooVetGit/Roo-Code） | README："Your AI-Powered Dev Team, Right in Your Editor"；"Roo Code gives you a whole dev team of AI agents in your code editor."；官网（Roomote）："The cloud coding agent you actually own. Roomote runs your actual dev environment, verifies its work, reviews the code with a different model, and hands back PRs with live previews for you to approve." | 编辑器里的一整支 AI 开发团队；云代理 Roomote 运行真实开发环境、跨模型复核代码、交付带实时预览的 PR 供审批 | 24,322★（新 org，2026-08-29）；**注意："已关停"说法不准确**，VS Code 扩展仍在维护，官网重心转向 Roomote |
| **Gemini CLI** | 官方仓库 [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli)、官方文档 [google-gemini.github.io/gemini-cli](https://google-gemini.github.io/gemini-cli)（Google） | 官方文档站："Gemini CLI is an open-source AI agent that brings the power of Gemini directly into your terminal." | 开源 AI 智能体，把 Gemini 的能力带进终端；内置 Google Search grounding、文件/Shell/网页工具 + MCP | 107k★（2026-08-29） |
| **gpt-pilot** | 官方仓库 [Pythagora-io/gpt-pilot](https://github.com/Pythagora-io/gpt-pilot)（Pythagora） | 仓库描述："The first real AI developer." | 第一个真正的 AI 开发者（早期从想法生成完整应用的 pilot 项目） | 34k★ |

### ④ 纯 vibe coding 平台（简要）

| 工作流 | 官方来源 | 官网自我定义（原文） | 中文释义 | 热度 |
|--------|----------|----------------------|----------|------|
| **Bolt** | [bolt.new](https://bolt.new)、官方仓库 [stackblitz/bolt.new](https://github.com/stackblitz/bolt.new) | 仓库描述："Prompt, run, edit, and deploy full-stack web applications." | 提示、运行、编辑、部署全栈 Web 应用 | 17k★ |
| **v0** | [v0.dev](https://v0.dev)（Vercel） | "Prompt. Build. Publish. Generate working applications in minutes with AI. Publish as live websites in seconds." | 提示 → 构建 → 发布；几分钟生成可用应用，几秒发布成在线站点 | 闭源 |
| **Lovable** | [lovable.dev](https://lovable.dev)、官方文档 [docs.lovable.dev](https://docs.lovable.dev/introduction/welcome.md) | 官方文档："Lovable is a full-stack AI development platform for building, iterating on, and deploying web applications using natural language, with real code, security, and enterprise governance." | 全栈 AI 开发平台：自然语言构建、迭代、部署 Web 应用（真实代码、安全、企业治理） | 闭源 |

### ⑤ 规划/编排层与其他

| 工作流 | 官方来源 | 官网自我定义（原文） | 中文释义 | 热度/状态 |
|--------|----------|----------------------|----------|----------|
| **Vibe Kanban** | 官方仓库 [BloopAI/vibe-kanban](https://github.com/BloopAI/vibe-kanban)、官网 [vibekanban.com](https://vibekanban.com) | README："Get 10X more out of Claude Code, Gemini CLI, Codex, Amp and other coding agents… Use kanban issues to plan work, either privately or with your team. When you're ready to begin, create workspaces where coding agents can execute." | 看板规划 + agent workspace 执行：从 Claude Code / Codex 等智能体身上榨出 10 倍产出 | 27,949★；npm 7,494/月；**⚠️ 官方 README 头条 "Vibe Kanban is sunsetting"**（转社区维护，公告 vibekanban.com/blog/shutdown） |
| **Kiro（agentic IDE）** | 官网 [kiro.dev](https://kiro.dev)、[kiro.dev/about](https://kiro.dev/about/)、介绍博客 [kiro.dev/blog/introducing-kiro](https://kiro.dev/blog/introducing-kiro/) | "Kiro is an agentic development environment that makes it easy for developers to ship real engineering work with the help of AI agents."；"an AI IDE that helps you deliver from concept to production through a simplified developer experience for working with AI agents." | agentic 开发环境：借助 AI 智能体轻松交付真实工程工作；从概念到生产。**2025-11 AWS 宣布 Kiro GA**，成为 AWS 推出的 agentic IDE | 闭源；AWS 官方指定为 Amazon Q Developer 继任方向 |
| **Amazon Q Developer** | 官方产品页 [aws.amazon.com/q/developer](https://aws.amazon.com/q/developer/)、官方 EOL 公告 [aws.amazon.com/blogs/devops](https://aws.amazon.com/blogs/devops/amazon-q-developer-end-of-support-announcement/) | 产品页："The most capable generative AI–powered assistant for software development."；EOL 通知："On April 30, 2027, AWS will discontinue support for Amazon Q Developer IDE plugins. For capabilities similar to Amazon Q Developer IDE plugins, explore Kiro to access the latest models and features, including agentic coding, chat and MCP support." | 最强的生成式 AI 软件开发助手；**2027-04-30 起 IDE 插件 EOL**，官方引导转向 Kiro（含 agentic coding、chat、MCP） | 闭源；**EOL 状态（课程需注意）** |

---

## 四、纵向表：各工作流之间的区别

| 维度 | Superpowers | mattpocock/skills | Spec Kit | GSD (Core) | BMAD | Claude Code | Codex | OpenCode | Cursor | Windsurf/Cascade | Copilot (agent/coding) | Cline | Aider | Vibe Kanban |
|------|-------------|-------------------|----------|------------|------|-------------|-------|----------|--------|------------------|------------------------|-------|-------|-------------|
| **定位** | 完整方法论（拥有整个流程） | 可组合技能库（你拥有流程） | 规格驱动工作台（SDD，GitHub 官方） | 元提示 + 上下文工程 + SDD | 敏捷 AI 驱动开发方法 | 工具内建工作流 | 工具内建工作流 | 开源终端/IDE 代理 | IDE Agent + Plan/Subagents/Automations | Cascade Markdown 工作流 | GitHub 原生 Issue→PR 代理 | 开源 IDE/终端代理 | 终端结对编程 | 看板编排多代理 |
| **作者/公司** | Jesse Vincent (obra) | Matt Pocock / AI Hero | GitHub 官方（Den Delimarsky） | TÂCHES / Open GSD 社区 | BMad Code 组织 | Anthropic | OpenAI | Anomaly（原 SST） | Anysphere | 原 Codeium，现 Cognition（Devin） | GitHub (Microsoft) | Cline 社区 | Paul Gauthier 等 | BloopAI |
| **流程所有权** | 流程拥有你（强制） | 你拥有流程（可改造） | 流程可选（自带或自定义） | 流程驱动（纪律化阶段循环） | 流程按需缩放（小改直通） | 用户编排 | 用户编排 | 用户编排（plan/build） | 用户编排 + 团队 Automations | 用户编排（Markdown 工作流） | GitHub 编排 | 用户编排 | 用户编排 | 用户编排（看板） |
| **触发/注入机制** | session-start hook 强制注入 | 无 hooks，用户显式调用 + 模型按需触发 | 结构化 spec 六阶段 slash 命令（constitution→specify→plan→tasks→implement→converge） | 五步循环（Discuss→Plan→Execute→Verify→Ship）+ 全新上下文子代理 | 命令式（bmad-build），Clarify/Plan/Build 分诊 | 对话 + plan mode + subagents + hooks + dynamic workflows | 对话 + workflows + automations + plan mode | plan/build 双 agent + subagents + MCP | Plan Mode + Subagents + Automations + Cloud Agents | Markdown 工作流文件 + 斜杠命令 | Issue/PR 事件触发 | 对话 + 计划模式 | 对话 + 自动 git | 看板卡片触发 |
| **自主程度** | 极高（每任务全新子代理，数小时自治） | 中（人工闸门多） | 中高（阶段门控，可自动化） | 高（执行子代理独立上下文） | 中（决策留给人） | 中高（hooks 可自动化） | 中高（automations） | 中 | 中（人工确认 + 云端自动化） | 中 | 中高（PR 迭代闭环） | 高（自主任务） | 低（结对模式） | 中（人规划+评审） |
| **上手方式** | 大爆炸（一次接受整套方法论） | 渐进（一课一技能） | 渐进（先 spec 再跑流程） | 渐进（npx 安装 + 阶段循环） | 渐进（npx bmad-method install） | 即装即用 | 即装即用 | 即装即用 | 即装即用 | 即装即用 | GitHub 内即用 | 即装即用 | 即装即用 | 即装即用 |
| **教学属性** | 无内置教学技能 | 自带 /teach、作者是教育家 | 无 | 无 | 无 | 官方 docs 全面 | 官方 Cookbook/Academy | 官方 docs | 官方 docs | 官方 docs | 官方 docs/blog | 社区 docs | 官方 docs | 社区 |
| **对 vibe coding 的态度** | 反（纪律替代随性） | 反（"not vibe coding"，点名批评 GSD/BMAD/Spec-Kit） | 中立偏纪律（SDD） | 纪律化（spec 驱动） | 纪律化（保留人的思考） | 纪律导向（workflows 文档） | 纪律导向 | 中立 | 中立 | 中立 | 纪律导向（PR 闭环） | 中立 | 中立 | 中立（强调人审） |
| **热度（2026-08-29）** | 279k★ | 240k★（19.0M installs） | 132k★ | 8.8k★（新仓库，npm 43.7k/月） | 52k★（npm 81.5k/月） | 143k★ | 120k★ | 202k★ | 闭源 | 闭源 | 闭源 | 67k★ | 48.5k★ | 28k★ |
| **当前状态** | 活跃 | 活跃 | 活跃（GitHub 官方维护） | 活跃（迁移后） | 活跃 | 活跃 | 活跃 | 活跃 | 活跃 | 并入 Devin | 活跃 | 活跃 | 活跃 | **sunsetting→社区** |

---

## 五、用户猜测核实结论（重要更正）

| 猜测/传言 | 核实结果 | 依据 |
|-----------|----------|------|
| Spec Kit 作者是 "crystal"？ | ❌ 创建者为 **Den Delimarsky**（2025-08-21 首次提交），现为 GitHub 官方 org 仓库 | github/spec-kit 提交历史；README 致谢 John Lam |
| GSD = "GitHub Standard"？gsd.dev？ | ❌ 全称 **Get Shit Done（现 Git. Ship. Done. / GSD Core）**，作者 TÂCHES / Open GSD；`gsd.dev` 域名不存在 | github.com/gsd-build/get-shit-done 、github.com/open-gsd/gsd-core 、opengsd.net |
| BMAD = "Basecamp-style Model of Agentic Development"？bmad.dev？ | ❌ 官方全称 **Breakthrough Method for Agile AI Driven Development**；`bmad.dev` 域名不存在，官网为 docs.bmad-method.org | github.com/bmad-code-org/BMAD-METHOD 、docs.bmad-method.org |
| spec-kit.dev？ | ❌ 域名不存在；官方文档站为 **github.github.io/spec-kit/** | curl 实测 + README 徽章 |
| Windsurf 状态 | ✅ **2025-07 被 Cognition（Devin）收购**，Cascade 工作流仍为官方功能 | cognition.ai/blog/windsurf 、windsurf.com/blog/windsurfs-next-chapter |
| Roo Code "已关停" | ⚠️ 不准确：VS Code 扩展仍在维护；官网重心转向云代理 **Roomote** | github.com/RooCodeInc/Roo-Code README 、roocode.com |
| Amazon Q Developer 现状 | ⚠️ **2027-04-30 IDE 插件 EOL**，官方引导转向 **Kiro** | aws.amazon.com/q/developer/ 、官方 EOL 公告 |
| Vibe Kanban 状态 | ⚠️ 官方 README 宣布 **sunsetting**，转社区维护 | github.com/BloopAI/vibe-kanban 、vibekanban.com/blog/shutdown |

---

## 六、关键发现与对课程的启示

### 1. 热度格局（2026-08-29）

- **方法论三巨头**：Superpowers（279k★）、mattpocock/skills（240k★、19.0M installs）、Spec Kit（132k★，已归 GitHub 官方组织）。
- **开源终端代理热度最高**：OpenCode（202k★）、Claude Code（143k★）、Codex（120k★）、Gemini CLI（107k★）。
- **闭源平台无法用 star 衡量**：Cursor、Windsurf（并入 Devin）、Copilot、Replit Agent、Devin、Kiro。

### 2. 生态变动（课程里值得讲，避免教过时内容）

1. **GSD 已迁移**：旧仓库 `gsd-build/get-shit-done`（64.6k★）归档 → 新家 `open-gsd/gsd-core`（"Git. Ship. Done."，opengsd.net）。
2. **Windsurf 并入 Devin**：Cognition 收购，官方公告后文档迁入 docs.devin.ai。
3. **Roo Code 转向 Roomote**：VS Code 扩展仍在，官网重心转向云代理 Roomote。
4. **Vibe Kanban sunsetting**：官方宣布转社区维护。
5. **Amazon Q Developer EOL**：2027-04-30 IDE 插件停服，官方指定继任者 Kiro（AWS 版 agentic IDE）。
6. **Spec Kit 官方化**：由 GitHub 官方组织接管并发布 1.0.0——规格驱动开发成为官方认可路线。

### 3. 对课程大纲的启示（衔接 6 月内部培训 → Superpowers → AI Coding for Real Engineers）

- **内部培训段（6 月）**：以工具内建工作流（Claude Code / Codex / OpenCode）为主，讲"工具怎么用"。
- **课程中段**：引入方法论文架，先讲 **mattpocock/skills**（渐进、可组合、人在回路），再讲 **Superpowers**（强制方法论、自治开发），保留"你拥有流程 vs 流程拥有你"的对照。
- **课程进阶/延伸**：用 **Spec Kit**（GitHub 官方）讲 Spec-Driven Development 第三条路线；用 **GSD Core / BMAD** 作其他方法论横向对照；用 OpenCode/Cline/Aider/Gemini CLI 作开源工具生态补充；用 Windsurf→Devin、Roo Code→Roomote、Vibe Kanban sunsetting、Amazon Q→Kiro 讲**生态变动与选型风险**。
- **课程作业建议**：让学员在同一个任务上分别跑"聊天式编程 / mattpocock/skills / Superpowers / Spec Kit"四种工作流，对比产出质量与可控性——这是两类表背后最直接的教学实验。

---

## 七、参考来源（均为官网 / 官方 GitHub / 官方文档）

**流程方法论类**
- Superpowers：https://github.com/obra/superpowers
- mattpocock/skills：https://github.com/mattpocock/skills 、https://aihero.dev/skills 、https://skills.sh/mattpocock/skills
- Spec Kit：https://github.com/github/spec-kit 、https://github.github.io/spec-kit/
- GSD Core：https://github.com/open-gsd/gsd-core 、https://opengsd.net 、旧仓库 https://github.com/gsd-build/get-shit-done
- BMAD：https://github.com/bmad-code-org/BMAD-METHOD 、https://docs.bmad-method.org 、https://bmadcode.com
- AGENTS.md：https://github.com/agentsmd/agents.md 、https://agents.md

**工具内建工作流类**
- Claude Code：https://code.claude.com/docs/en/common-workflows 、/en/permission-modes 、/en/sub-agents 、/en/hooks 、/en/workflows 、https://github.com/anthropics/claude-code
- OpenAI Codex：https://developers.openai.com/codex/ 、https://developers.openai.com/codex/codex-manual.md 、https://developers.openai.com/codex/app/automations 、https://developers.openai.com/cookbook/examples/codex/iterating-development-workflows-with-codex 、https://academy.openai.com/public/clubs/builders-etkn1/resources/codex-102-practical-workflows-2026-03-18 、https://github.com/openai/codex（注：`/codex/workflows/` 路径因站点反爬未能逐字验证，已如实标注）
- OpenCode：https://opencode.ai 、https://opencode.ai/docs/agents/ 、https://github.com/anomalyco/opencode
- Cursor：https://cursor.com/docs/agent/overview 、https://cursor.com/docs/agent/plan-mode 、https://cursor.com/docs/subagents 、https://cursor.com/docs/cloud-agent/automations
- Windsurf：https://docs.windsurf.com/windsurf/cascade/workflows 、https://cognition.ai/blog/windsurf 、https://windsurf.com/blog/windsurfs-next-chapter
- GitHub Copilot：https://github.blog/ai-and-ml/github-copilot/agent-mode-101-all-about-github-copilots-powerful-mode/ 、https://github.blog/ai-and-ml/github-copilot/github-copilot-coding-agent-101-getting-started-with-agentic-workflows-on-github/ 、https://docs.github.com/en/copilot
- Replit Agent：https://replit.com/agent 、https://docs.replit.com/replitai/agent

**自治代理类**
- Devin：https://devin.ai 、https://docs.devin.ai/get-started/devin-intro 、https://cognition.ai/blog/introducing-devin
- Cline：https://cline.bot 、https://github.com/cline/cline
- Aider：https://aider.chat 、https://github.com/Aider-AI/aider
- Roo Code：https://roocode.com 、https://github.com/RooCodeInc/Roo-Code
- Gemini CLI：https://github.com/google-gemini/gemini-cli 、https://google-gemini.github.io/gemini-cli/
- gpt-pilot：https://github.com/Pythagora-io/gpt-pilot

**纯 vibe coding 平台**
- Bolt：https://bolt.new 、https://github.com/stackblitz/bolt.new
- v0：https://v0.dev
- Lovable：https://lovable.dev 、https://docs.lovable.dev/introduction/welcome.md

**规划/编排层与其他**
- Vibe Kanban：https://github.com/BloopAI/vibe-kanban 、https://vibekanban.com
- Kiro：https://kiro.dev 、https://kiro.dev/about/ 、AWS 官方日文博客 https://aws.amazon.com/jp/blogs/news/introducing-kiro/ 、AWS 官方 What's New（Kiro 上线 GovCloud）https://aws.amazon.com/fr/about-aws/whats-new/2026/02/kiro-launch-aws-govcloud-us/ 、AWS 官方 What's New（Kiro 新模型上线）https://aws.amazon.com/cn/about-aws/whats-new/2026/06/kiro-gpt-nemotron-launch-aws-govcloud-us/
- Amazon Q Developer：https://aws.amazon.com/q/developer/ 、https://aws.amazon.com/blogs/devops/amazon-q-developer-end-of-support-announcement/

> **数据实测说明**：star/fork 与 npm 下载量为 2026-08-29 当日 GitHub REST API / npm registry / 官方页面实测；skills.sh 安装量为该日官方页面显示；Windsurf 收购、Roo Code 转向、Vibe Kanban sunsetting、Amazon Q EOL 均以官方页面/官方仓库公告为准。
