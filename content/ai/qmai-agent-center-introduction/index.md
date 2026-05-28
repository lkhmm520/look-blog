---
title: "Qmai Agent Center：前端 AI 工作流规则中心"
date: 2026-05-28T00:00:00+08:00
draft: false
summary: "本文介绍 Qmai Agent Center 的定位、核心功能、运作机制和团队价值。它是一套面向 Codex、Cursor、Copilot 等 AI 编码工具的前端工作流规则中心，用来让 AI 自动识别企迈国际化前端项目、选择正确 playbook，并按团队规范完成分析、修改、验证和交付。"
description: "Qmai Agent Center 是企迈国际化前端团队的 AI 编码工作流规则中心，提供项目自动识别、任务自动路由、全局开发约束、场景化 playbook、UI 规范落地和多工具接入能力。"

cover:
  image: "cover.png"
  alt: "Qmai Agent Center 前端 AI 工作流规则中心"
  relative: true

tags: ["Qmai Agent Center", "AI Coding", "前端工程", "工作流治理"]
categories: ["AI 工程化", "前端工程"]
---

## 一句话介绍

`Qmai Agent Center` 是企迈国际化前端团队的 AI 编码工作流规则中心。

它不是一个业务系统，也不是普通文档仓库，而是一套给 Codex、Cursor、Copilot 等 AI 编码工具使用的规则与流程中枢。它的目标是让 AI 进入任意 Qmai 国际化前端项目后，能够自动识别项目类型、选择正确工作流，并按团队规范完成需求分析、代码修改、验证和交付。

简单说，它让 AI 从“会写代码”变成“懂企迈国际化前端工作流，并按团队规则做事”。

## 为什么需要它

企迈国际化前端不是单一项目，而是一组多仓库协作的系统，里面包含 Vue2 后台子应用、qiankun 基座、共享组件库、公共业务页、打印能力、UI 规范和预发布流程。

如果没有统一规则，AI 在处理需求时容易出现这些问题：

- 不知道当前项目属于哪个业务域
- 分不清 qiankun 基座和子应用的职责边界
- 遇到共享组件问题时只在业务仓库里打补丁
- 打印需求分不清模板逻辑和运行时逻辑
- 新页面开发忽略 UI 规范、i18n 和 RTL
- 发布相关问题误走正式环境或错误分支
- 每个 AI 工具、每个人的提示词口径不一致

`Qmai Agent Center` 的作用，就是把这些团队经验沉淀成 AI 可以读取、可以执行、可以持续维护的规则资产。

## 核心功能

### 1. 项目自动识别

AI 进入任意前端项目后，会先根据当前项目自身的信息进行判断，而不是依赖固定本机路径。

它会重点识别：

- 当前项目是否属于 Qmai / Qimai 体系
- 是否是 Vue2 后台项目
- 是否涉及 qiankun 基座或子应用
- 是否依赖 `@qmai/*`、`@qmai/vue-kylin`、Element UI 等体系能力
- 是否需要联查 `console-vue-international`、`vue-kylin-international`、`common-vue-international` 等相关仓库

这可以避免 AI 一上来就猜路径、猜项目、猜仓库职责。

### 2. 任务自动路由

项目识别之后，AI 会根据任务类型选择对应的工作流。

常见路由包括：

- 普通 Vue2 后台子应用开发：进入 `international-vue-subapp`
- 需求分析到实现交付的完整链路：进入 `international-delivery-workflow`
- Feishu 需求、工单或需求文案解析：进入 `international-demand-parser`
- qiankun 基座、菜单、登录、容器布局：进入 `console-vue-international`
- 公共业务页、打印模板、公共配置页：进入 `common-vue-international`
- 打印运行逻辑、打印桥接、打印插件：进入 `print-component`
- 共享 UI 组件、样式一致性、组件行为：进入 `vue-kylin-international`
- UI / UX、页面布局、设计 token、新后台页面：进入 `international-design-technical-spec`
- `pre` 发布、发布状态检查、OPMS 排障：进入 `shared-opms-release`

这样 AI 不再只靠临场判断，而是先进入正确的业务上下文。

### 3. 全局开发约束

`Qmai Agent Center` 统一约束 AI 的基础行为，避免它做出高风险操作。

例如：

- 修改前先说明目标、影响范围和步骤
- 多文件改动前先列出影响文件
- 不直接在 `pre` 分支上改需求代码
- 未经要求不执行 `git commit` 或 `git push`
- 不修改生产配置、不硬编码密钥或 token
- 可见文案必须考虑 i18n
- 样式改动必须考虑 RTL
- 接口路径包含 `/sellers` 时，默认按 `/assistant` 做代码映射
- 涉及 `sellerId` 时，默认不让前端重复透传

这些约束让 AI 的输出更接近团队真实开发规范。

### 4. 场景化 Playbook

`playbooks` 是这个项目最核心的执行层。

每个 playbook 都是一份场景化操作手册，告诉 AI 在某类任务中应该如何分析、如何定位、如何修改、如何验证。

目前覆盖的主要场景包括：

- 需求解析
- 完整交付流程
- 普通国际化 Vue 子应用开发
- qiankun 基座能力
- 公共业务页和打印模板
- 打印运行逻辑
- 共享组件库
- 后台 UI 设计规范
- OPMS 预发布排障

这些 playbook 的价值，是把团队经验从“靠人记住”变成“AI 每次都会读取并执行”。

### 5. UI 规范落地

项目内置了国际化后台页面的 UI 规范，覆盖页面结构、视觉层级、设计 token、表格、表单、按钮、状态、空态、弹窗、i18n 和 RTL。

当任务涉及新增后台页面、页面改造、视觉规范、设计 token 或 UI 一致性时，AI 会先读取 `international-design-technical-spec`。

这能帮助新页面保持统一的后台工作台风格，避免每个业务仓库各自发明一套页面结构。

### 6. 多工具接入

`Qmai Agent Center` 不绑定某一个 AI 工具。

它通过 `INSTALL.md` 和 `templates/` 提供接入方式，可以被 Codex、Cursor、Copilot 等工具复用。

团队只需要维护同一套规则中心，就可以让不同 AI 编码工具按统一规范工作。

## 运作机制

它的工作链路可以概括为：

```text
AI 进入项目
  ↓
读取入口规则
  ↓
加载全局约束
  ↓
识别当前项目类型
  ↓
根据任务选择 playbook
  ↓
按场景手册完成分析、修改、验证
  ↓
输出改动说明、验证结果和需确认事项
```

从代码结构上看，主要分成三层：

| 层级 | 目录 | 职责 |
| --- | --- | --- |
| 规则层 | `rules/` | 全局约束、项目识别、任务路由 |
| 执行层 | `playbooks/` | 各类任务的具体执行手册 |
| 接入层 | `templates/`、`INSTALL.md`、`AGENTS.md` | 接入 Codex、Cursor、Copilot 等 AI 工具 |

其中，`rules/workflow-router.mdc` 是唯一权威路由源，负责决定任务应该进入哪个 playbook；`playbooks/INDEX.md` 只做目录索引；`playbooks/CONVENTIONS.md` 负责维护 playbook 的命名和职责边界。

## 典型使用场景

### 新增一个后台页面

AI 会先识别目标仓库是否是 Qmai 国际化 Vue 子应用，再判断是否需要 UI 规范。如果任务涉及页面布局、视觉层级或设计 token，会先读取 UI 设计规范，再回到对应子应用 playbook 完成实现。

### 修改菜单、登录或容器布局

这类问题通常不只属于业务子应用。AI 会判断是否涉及 qiankun 基座，并进入 `console-vue-international` 相关 playbook，避免只在子应用里做局部修补。

### 调整共享组件行为

如果问题来自 `@qmai/vue-kylin` 或多个业务仓库重复出现的组件表现，AI 会优先评估共享组件库，而不是在多个业务项目里复制补丁。

### 处理打印需求

打印相关需求会先区分职责归属。打印模板和纸面内容布局通常属于 `common-vue-international`；打印运行逻辑、本地桥接和插件调试属于 `print-component`。

### 处理发布排障

如果任务已经进入发布或 OPMS 排障阶段，AI 会进入 `shared-opms-release`，并遵守只处理 `pre` 发布、不触发正式环境发布的约束。

## 带来的价值

### 1. 提升 AI 交付稳定性

AI 不再只靠临时提示词工作，而是每次都按统一流程识别项目、选择工作流、检查约束。

### 2. 降低跨仓协作风险

对于 qiankun、共享组件、打印、公共页面等跨仓场景，规则中心会提醒 AI 判断上下游影响，减少误改和漏查。

### 3. 统一团队规范

分支、发布、接口、UI、i18n、RTL、安全等规范都集中沉淀，减少不同人、不同工具之间的执行偏差。

### 4. 沉淀团队经验

过去依赖个人经验判断的流程，可以逐步沉淀为 playbook，让团队知识变成可维护、可复用、可迭代的资产。

### 5. 方便持续扩展

后续如果新增业务仓库、新流程、新 UI 规范或新发布规则，只需要扩展对应规则和 playbook，就能让所有接入的 AI 工具同步受益。

## 当前建设状态

目前项目已经完成基础结构治理：

- `rules/workflow-router.mdc` 作为唯一任务路由真源
- `playbooks/INDEX.md` 作为 playbook 索引
- `playbooks/CONVENTIONS.md` 作为目录命名和职责治理规范
- `playbooks/*/PLAYBOOK.md` 作为具体场景执行手册
- `playbooks/scripts/` 作为共享辅助脚本目录
- UI 规范预览分为 `embedded-preview` 和 `standalone-preview-app`

这意味着它已经具备作为团队 AI 工作流规则中心继续扩展的基础。

## 总结

`Qmai Agent Center` 的核心价值，不是让 AI 多读几份文档，而是给 AI 建立一套稳定的工作机制。

它把企迈国际化前端的项目识别、任务路由、开发约束、UI 规范、跨仓判断和发布流程整合到一起，让 AI 在真实业务项目里更像一个了解团队规则的协作者，而不是一个只会生成代码的工具。
