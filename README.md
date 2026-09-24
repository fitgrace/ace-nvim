# AceNvim

> A clear, stable and maintainable Neovim environment.

以 **Feature-Centric（以功能为中心）** 作为核心架构，遵循 **Native First（原生优先）** 设计原则，旨在打造简洁、清晰、稳定、易维护、可长期演进的现代化 Neovim 开发环境。

**结构清晰・行为可预期・低认知负担・可观测・IDE 级体验**

## 项目状态

项目目前处于设计准备阶段，尚未实现可用的开发配置。仓库中的 init.lua 为空入口，Core 和 Feature 尚未实现。下文描述的是设计方向与规划结构。

## 设计方向

AceNvim 面向长期使用 Neovim 的开发者，目标是提供一套稳定、取舍明确的默认开发体验；同时保证整个配置体系结构清晰、可观测、易于维护。

## 规划结构

目录结构遵循 Feature-Centric 原则，会随项目迭代持续优化。

```text
AceNvim/
│
├── init.lua
├── lua
│   ├── core/
│   │   ├── bootstrap.lua
│   │   ├── options.lua
│   │   ├── keybinds.lua
│   │   └── autocmds.lua
│   └── features/
│
├── docs/
│   ├── agents/
│   └── adr/
│
├── AGENTS.md
├── CONTEXT.md
└── README.md
```

> 以上为规划结构，不表示相关模块已经实现。目录和模块按实际需求逐步添加，职责与依赖规则见架构文档。

## 项目文档

- [核心原则](docs/principles.md)：设计取舍的依据。
- [目标架构](docs/architecture.md)：职责边界与依赖关系。
- [Agent 工作规则](AGENTS.md)：项目内的协作与修改约束。
