# AGENTS.md

> AGENTS.md 只回答：Agent 在这个项目里应该如何工作，以及哪些事情绝对不能做。

## Project

AceNvim 是面向 Neovim 0.12+ 的现代化个人开发环境。

项目采用 Feature-Centric（以功能为中心） 架构，遵循 Native First（原生优先） 原则，追求简洁、清晰、稳定、易维护和可长期演进。

## Core Principles

AI Agent 必须遵循：

- `docs/principles.md` 定义的核心原则
- `docs/architecture.md` 定义的目标架构契约

## Engineering Rules

- 先定义需求，再选择技术方案。
- 优先使用 Neovim 原生能力。
- 新增插件必须有明确的能力缺口和引入理由。
- 按 Feature 组织代码，保持 Feature 边界。
- 优先使用局部状态，避免不必要的全局状态。
- 对外部依赖和不稳定边界进行显式错误处理。
- 新增抽象必须解决真实问题。
- 优先保持 Feature 可独立删除。
- 不为假设的未来需求提前设计。
- 优先选择简单、可维护的实现。

## Workflow

所有任务遵循：

- 先理解需求和现有实现。
- 再检查相关代码、Feature 和依赖。
- 修改前形成简洁计划。
- 按最小必要范围实施。
- 完成后进行验证。
- 最后检查架构、行为和文档影响。

### Issue tracker

需要落盘的需求规格与实施任务使用本地 Markdown 存放于 `.scratch/<feature-slug>/`。

文件组织与相关工作流约定见 `docs/agents/issue-tracker.md`。

## Before Coding

编码前必须：

- 阅读 AGENTS.md。
- 阅读与任务相关的项目文档。
- 检查相关 Feature 的现有实现。
- 确认是否已有可复用能力。
- 确认是否需要新增依赖。
- 确认修改不会破坏现有架构边界。

涉及架构的任务，应阅读：

- docs/principles.md
- docs/architecture.md

## Coding Rules

- 优先使用 Neovim 原生 API。
- 优先保持代码简单、显式、局部。
- Feature 不得直接依赖其他 Feature 的内部实现。
- 避免不必要的 vim.g 和其他全局状态。
- 避免隐藏初始化和隐式依赖。
- 避免无意义的 Wrapper、Manager、Registry 等抽象。
- 外部依赖不可用时，应尽可能提供合理降级。
- 模块和函数保持清晰、单一职责。
- 不为了追求“架构完整”而增加复杂度。

## Change Rules

- 只修改完成任务所需的最小范围。
- 不顺带修改无关代码。
- 不顺带进行无关重构。
- 不擅自改变已有行为。
- 新增或替换插件时，说明原因及影响。
- 涉及 Feature 边界、依赖方向或 Core 的修改，视为架构变更。
- 发现与当前任务无关的架构问题时，提出建议，不擅自扩大任务范围。
- 行为、架构或决策发生变化时，同步相关文档。

## Validation

纯文档变更检查规则一致性、引用有效性和条件可判定性；运行代码变更才执行适用的 Neovim 启动及功能验证。

代码变更至少确认：

- Neovim 可以正常启动
- 相关 Feature 正常工作
- 没有明显 Lua / runtime 错误

涉及 Feature、插件或架构修改时，还应验证：

- 依赖是否正常；
- 错误处理是否合理；
- Feature 边界是否保持；
- 可选依赖缺失时是否能够降级；
- 是否影响其他 Feature。

## Documentation

项目知识按职责分层，不重复堆积。

- `README.md`：项目是什么。
- `AGENTS.md`：Agent 在这个项目里应该如何工作。
- `CONTEXT.md`：领域术语与业务语义，按需创建。
- `docs/principles.md`：核心原则唯一事实来源。
- `docs/architecture.md`：目标架构与模块关系。
- `docs/adr/`：具体架构决策及其理由，按需创建。

领域文档采用 single-context 布局，阅读、术语使用及决策冲突处理规则见 `docs/agents/domain.md`。

当代码、架构或工程规则发生变化时，检查相关文档是否需要同步更新。
