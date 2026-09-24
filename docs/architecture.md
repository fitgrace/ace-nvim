# ARCHITECTURE.md

> AceNvim 的架构定义系统的基本组成、职责边界和依赖关系。

## Architecture Model

AceNvim 采用 **Feature-Centric（以功能为中心）** 架构。

系统以用户工作流为基本组织单元，而不是以插件为组织中心。

```text
AceNvim
├── core
└── features
    ├── feature A
    ├── feature B
    └── feature C
```

* **Core**：提供系统级基础能力。
* **Feature**：提供独立的用户功能。

## Core

Core 负责 AceNvim 的基础运行环境，不选择、枚举或加载具体 Feature。

Core 可承担以下职责：

* 基础配置与初始化
* 有实际共享需求的基础工具
* 系统级事件协调
* 基础运行状态与错误反馈

以上是职责边界，不是必须逐项实现的模块清单。仅在具体需求出现时添加相应实现，不预建占位模块或统一框架。

实现时优先使用 Neovim 原生能力：

* 事件协调优先使用原生 autocmd；有明确需求时使用 User 事件，不预建通用事件总线。
* 运行状态与错误反馈优先使用原生通知、消息及健康检查机制，不预建统一诊断系统。
* 基础工具只有在实际复用需求明确、且职责属于 Core 时才提取；Feature 专属逻辑保留在 Feature 内部。

Core 应保持轻量、稳定，不应成为所有 Feature 的公共代码仓库。

> Core 提供基础设施，Feature 提供用户能力。

## Feature

Feature 是 AceNvim 的基本架构单元。

一个 Feature 应代表一个相对独立的用户功能或工作流，例如：

```text
Git
LSP
Search
```

Feature 应具有：

* 明确职责
* 明确入口
* 明确依赖
* 独立生命周期
* 局部状态

Feature 内部可以使用：

```text
Native API
Lua
Plugin
External Tool
```

但这些都是实现手段，而不是架构组织方式。

## Boundaries & Dependencies

Feature 同时是**职责边界和故障边界**。

```text
Core
  ↑
  │
Feature
  │
  ├── Native
  ├── Plugin
  └── External Tool
```

基本规则：

* Core 不依赖具体 Feature：不引用具体 Feature 模块，也不包含基于 Feature 名称的特殊处理。该限制不适用于负责启动组合的 `init.lua`。
* Feature 不依赖其他 Feature 的内部实现。
* 可选依赖不能成为系统启动的必要条件。
* Feature 之间通过明确接口或事件协作。
* Feature 的故障处理应满足 Failure Isolation 中定义的范围和验收要求。
* `init.lua` 是启动组合入口，显式选择需要启用的 Feature，并组织初始化调用。它可以依赖 Core 和具体 Feature 的公开入口，但不承载 Feature 内部实现。

> 某一个 Feature 删除后，应尽可能不影响其他 Feature。

## Failure Isolation

Feature 的故障隔离目标是：单个 Feature 失败后，基础编辑能力仍可用，其他独立 Feature 能继续初始化和使用。故障隔离不意味着自动回滚全部副作用。

每个 Feature 实现时，应明确以下失败场景的处理与验证方式；不适用的场景应注明原因。

### 加载与初始化失败

* Feature 模块加载阶段仅定义能力，不创建运行副作用；映射、命令、autocmd 和任务等在显式初始化入口内创建。
* 初始化失败的 Feature 不应被报告为可用。
* Feature 应先检查必要条件，再创建映射、命令、autocmd 等副作用。

### 异步与延迟回调失败

* 启动入口的异常保护不覆盖初始化返回后执行的回调。
* Feature 负责其异步任务、定时器及事件回调中的错误处理。
* 回调失败后，应终止本次失败操作，并根据影响停止相关任务、禁用受影响能力或保留可用部分。
* 不得静默吞掉错误，也不得持续重试形成重复报错；重试必须有明确的触发条件或次数限制。

### 部分初始化与副作用

* Feature 负责清理自身在失败的初始化过程中创建的资源，例如 autocmd、命令、映射、定时器和任务。
* 无法安全撤销的副作用，应在实现说明中列明，并说明失败后如何恢复。
* 清理不得删除其他 Feature 或用户已有的资源。
* 未完成清理时，不应盲目重新初始化。

### 错误反馈

* 错误反馈应包含 Feature 名称、失败操作或阶段、原因，以及可行的恢复建议。
* 可选依赖缺失时，应说明哪些能力不可用、哪些能力仍可使用。
* 优先使用 Neovim 原生反馈机制，不要求预建统一错误管理系统。

## Directory Structure

推荐结构：

```text
.
├── init.lua
├── lua/
│   ├── core/
│   └── features/
│       ├── search/
│       ├── lsp/
│       └── git/
├── docs/
└── AGENTS.md
```

其中：

```text
core/
```

负责系统基础能力。

```text
features/
```

负责用户功能。

## Runtime Flow

启动入口先初始化 Core，再初始化选定的 Feature。

Core 初始化完成后，启动入口调用各 Feature 的公开初始化入口。启动入口负责隔离单个 Feature 的加载或初始化异常，并报告失败；Feature 负责其内部可选依赖的检查与降级。仅为组织启动，不引入自动扫描、注册中心或依赖排序机制。

Feature 自己负责其功能的初始化和生命周期。

避免建立大型中央 Manager、复杂 Feature Registry 或其他非必要的中间层。

## Architectural Goal

AceNvim 的架构目标不是构建复杂的 Neovim Framework，而是建立：

> **结构清晰、边界明确、依赖可控、故障隔离、能够长期维护的现代化 Neovim 开发环境。**

架构复杂度应由真实需求驱动，而不是由对未来的假设驱动。
