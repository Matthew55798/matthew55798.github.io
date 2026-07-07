# AI Coding 团队实践指南调研

**创建时间：** 2026-06-30
**当前阶段：** 学习与总结

## 目标

沉淀一套面向企业级应用开发团队的 AI Coding 实践指南。重点不是工具评测，也不是泛泛介绍 vibe coding，而是把 AI Coding 纳入团队研发流程，覆盖需求沟通、需求澄清、技术设计、任务拆分、编码实现、测试验证、代码评审、文档沉淀和持续改进。

## 调研范围

### 项目类型

- 后端：Java 后端、Spring Boot 微服务、企业自研后端框架。
- 前端：Vue 项目，以及企业内部可能使用的 C# 技术栈。
- 项目状态：重点关注非 AI native 项目，即缺少 AI 协作上下文、文档不完整、测试不充分、历史包袱较重的传统工程。

### 工具范围

保持方法论尽量工具无关，但调研时至少覆盖：

- Codex
- Claude Code
- OpenCode

## 核心问题

1. 既有项目如何引入 AI Coding，而不破坏原有架构、规范和交付稳定性？
2. 新项目从 0 到 1 时，如何用 AI 辅助完成需求、设计、编码、测试和文档闭环？
3. 团队需要准备哪些上下文资产，例如 `AGENTS.md`、`CLAUDE.md`、架构说明、模块说明、测试说明、Prompt 模板、Checklist？
4. 哪些任务适合交给 AI，哪些任务必须人类主导？
5. 如何把 AI 产出纳入现有工程控制，例如分支策略、代码评审、CI、单测、集成测试、安全扫描、发布回滚？
6. 如何衡量 AI Coding 是否真的提升了团队效率，而不是把成本转移到 review、返工和线上风险？

## 学习资料清单

### 一、Codex 官方资料

- [Prompting - Codex](https://developers.openai.com/codex/prompting)
  - 重点阅读：如何提供验证方式、如何拆小任务、如何让 Codex 先计划再实现。
- [Workflows - Codex](https://developers.openai.com/codex/workflows)
  - 重点阅读：端到端工作流示例，尤其是如何把任务交给 Codex 时定义上下文和完成标准。
- [Best practices - Codex](https://developers.openai.com/codex/learn/best-practices)
  - 重点阅读：计划优先、上下文准备、验证、MCP、skills、automations。
- [Custom instructions with AGENTS.md - Codex](https://developers.openai.com/codex/guides/agents-md)
  - 重点阅读：`AGENTS.md` 的层级、作用域、覆盖规则、验证方法。
- [Agent Skills - Codex](https://developers.openai.com/codex/skills)
  - 重点阅读：如何把重复工作流沉淀为 skill，而不是每次复制长 prompt。
- [Subagents - Codex](https://developers.openai.com/codex/subagents)
  - 重点阅读：复杂任务中如何拆分探索、实现、review 等角色。
- [Codex code review in GitHub](https://developers.openai.com/codex/integrations/github)
  - 重点阅读：AI review 与团队 review 规范如何结合。
- [Codex CLI features](https://developers.openai.com/codex/cli/features)
  - 重点阅读：本地 review、CLI 工作流、命令式协作方式。
- [Sandbox - Codex](https://developers.openai.com/codex/concepts/sandboxing)
  - 重点阅读：权限边界、沙箱和高风险操作控制。

### 二、Claude Code 官方资料

- [Claude Code common workflows](https://code.claude.com/docs/en/common-workflows)
  - 重点阅读：理解代码库、修 bug、重构、写测试、PR、文档、worktree 并行、先计划后编辑。
- [How Claude remembers your project](https://code.claude.com/docs/en/memory)
  - 重点阅读：`CLAUDE.md`、`AGENTS.md` 导入、memory、项目规则、团队级上下文管理。
- [Claude Code power user tips](https://support.claude.com/en/articles/14554000-claude-code-power-user-tips)
  - 重点阅读：计划、并行、自动化、验证、定制化；尤其是“给 Claude 自我验证手段”。
- [Extend Claude with skills](https://code.claude.com/docs/en/skills)
  - 重点阅读：何时把重复 prompt 升级为 skill，如何组织技能和验证。
- [Create custom subagents](https://code.claude.com/docs/en/sub-agents)
  - 重点阅读：代码评审、调试、探索等专用 agent 如何定义。
- [Automate actions with hooks](https://code.claude.com/docs/en/hooks-guide)
  - 重点阅读：自动格式化、阻止高风险命令、注入上下文、触发验证。
- [Claude Code GitHub Actions](https://code.claude.com/docs/en/github-actions)
  - 重点阅读：Issue / PR 驱动开发、CI 中的 AI 自动化、安全和成本控制。

### 三、OpenCode 官方资料

- [OpenCode Intro](https://opencode.ai/docs/)
  - 重点阅读：Ask questions、Add features、Plan mode、Build mode、Undo changes。
- [OpenCode Agents](https://opencode.ai/docs/agents/)
  - 重点阅读：Plan agent、Build agent、Subagents，以及如何用受限 agent 做只读分析。
- [OpenCode Config](https://opencode.ai/docs/config/)
  - 重点阅读：默认 agent、命令模板、共享策略、团队级配置。

### 四、通用工程实践资料

- [Google Engineering Practices: Code Review](https://google.github.io/eng-practices/review/)
  - 重点阅读：review 应检查设计、功能、复杂度、测试、命名、注释、风格、文档。
- [Google Engineering Practices: Small CLs](https://google.github.io/eng-practices/review/developer/small-cls.html)
  - 重点阅读：为什么 AI Coding 更需要小批量、可审查、可回滚的变更。
- [2025 DORA State of AI-assisted Software Development](https://cloud.google.com/resources/content/2025-dora-ai-assisted-software-development-report)
  - 重点阅读：AI 落地是系统问题，不只是工具问题；关注吞吐、稳定性、返工、价值流。
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
  - 重点阅读：Prompt Injection、Insecure Output Handling、Sensitive Information Disclosure、Excessive Agency、Overreliance。
- [GitHub Copilot repository custom instructions](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions)
  - 重点阅读：`AGENTS.md` 在不同 AI coding 工具中的通用趋势。
- [GitHub Copilot code review](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/request-a-code-review/use-code-review)
  - 重点阅读：AI review 如何作为额外检查层，而不是替代人类责任。
- [Responsible AI with GitHub Copilot](https://learn.microsoft.com/en-us/training/modules/responsible-ai-with-github-copilot/)
  - 重点阅读：AI 生成代码的责任、限制、透明度和项目约束。

### 五、本地资料

- [Claude Code 入门教程](/软件工程/技术/AI/菜鸟教程-AI%20Agent/第3章-Vibe%20Coding/3.1-Claude%20Code入门教程.md)
- [OpenCode 入门教程](/软件工程/技术/AI/菜鸟教程-AI%20Agent/第3章-Vibe%20Coding/3.2-OpenCode入门教程.md)
- [OpenCode Coding Plan](/软件工程/技术/AI/菜鸟教程-AI%20Agent/第3章-Vibe%20Coding/3.3-OpenCode%20Coding%20Plan.md)
- [OpenCode skills](/软件工程/技术/AI/菜鸟教程-AI%20Agent/第3章-Vibe%20Coding/3.6-OpenCode%20skills.md)

## 阅读记录模板

| 资料 | 适用场景 | 可复用做法 | 风险/限制 | 后续可转化内容 |
|------|----------|------------|-----------|----------------|
|  |  |  |  | Prompt / Checklist / 流程节点 / 团队规范 |

## 初步假设

1. AI Coding 的核心不是“让 AI 写更多代码”，而是把 AI 放进可验证的软件工程闭环。
2. 非 AI native 项目的第一步不是编码，而是补齐 AI 可读的工程上下文：项目结构、运行命令、测试命令、模块边界、业务词汇、禁止事项。
3. 团队级 AI Coding 必须小任务、小分支、小 PR、小 review，否则效率提升会被返工和风险吞掉。
4. Prompt 最终应沉淀为三类资产：一次性任务 prompt、项目级上下文文件、可复用 workflow/skill。
5. AI 适合做探索、草案、重复性实现、测试补齐、文档整理、迁移辅助；架构取舍、需求冲突、风险接受、最终合并责任仍必须由人承担。

## 下一步计划

1. 阅读资料并按“上下文、计划、实现、验证、review、团队治理”六个维度做摘要。
2. 选择一个已有项目做实践：先让 AI 只读分析，再做一个低风险 bugfix 或测试补齐。
3. 选择一个小型新项目做从 0 到 1 实践：先写需求和设计，再让 AI 分阶段实现。
4. 基于实践结果整理最终团队实践指南，包含流程、Prompt 模板、Checklist、反例和案例。
