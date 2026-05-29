---
title: "Qmai 全局约束"
description: "Qmai Agent Center 的全局行为约束：修改流程、代码质量、安全、沟通、Git、发布和 Feishu 规则。"
summary: "AI 在 Qmai 国际化前端项目中必须遵守的基础约束。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 20
tags: ["Qmai Agent Center", "AI Coding", "工作流规范"]
categories: ["AI 工程化"]
series: ["Qmai Agent Center"]
series_order: 2
---

本页整理自 `agent-center/rules/global-constraints.mdc`，是 `Qmai Agent Center` 的全局约束源。它定义 AI 在企迈国际化前端项目中做分析、改代码、验证、交付和沟通时必须遵守的底线。

## 修改流程

### 先出方案再动手

修改前需要先输出简要方案，至少包含：

- 修改目标
- 影响范围
- 修改步骤

如果任务涉及多文件或多模块，需要先列出完整的文件变更清单。除非用户明确要求直接修改，否则应先让用户知道准备怎么做。

### 最小闭环

每次修改都应追求可独立运行、可独立验证的最小闭环。

- 不一次性做大范围重构。
- 不把无关优化混进当前需求。
- 每个闭环都要有改动和自验方式。
- 没完成的路径不能留下半成品代码。

### 影响分析

修改公共模块、共享组件、API 调用或数据结构时，必须分析上下游影响。涉及跨仓库联动时，先判断当前工作区是否能访问相关仓库；如果看不到上下文，就明确说明缺少哪个仓库，而不是猜本机路径。

### qiankun 与子应用

Qmai Vue 后台项目默认先按 qiankun 基座与子应用关系分析。

- 菜单、登录、容器布局、微前端挂载、跨应用路由：判断是否涉及 `console-vue-international`。
- 共享 UI 组件、样式一致性、组件行为差异：判断是否涉及 `vue-kylin-international`。
- 公共业务页、`/commonCenter/`、打印模板：判断是否涉及 `common-vue-international`。

## 代码质量

### 风格一致

新代码应和当前文件、当前模块的风格保持一致，不主动引入新的范式、依赖或目录结构。发现旁路问题可以单独提出，但不应顺手改进。

### 改动最小

只修改与目标直接相关的代码，不做无关格式化、import 重排、空行清理或历史问题治理。这样可以降低评审成本，也避免把真实业务变更淹没在噪音里。

### sellerId 约束

当前所有 Qmai 项目中，涉及 `sellerId` 的接口入参、路由参数、函数透传参数时，默认不需要前端主动传递。

后端会从登录态 `token` 中自动解析并获取 `sellerId`。新增功能或接口对接时，不继续新增冗余 `sellerId` 传参，除非接口文档或后端明确说明该场景必须手动传。

历史代码已有的 `sellerId` 传参默认不主动治理；只有当前需求明确要求、联调确认必须调整，或该传参已导致实际问题时再处理。

## 安全与稳定性

### 不做破坏性变更

- 不删除已有对外暴露接口、组件、方法，除非用户明确要求。
- 重命名或移动文件时，同步更新所有引用。
- 修改组件 `props`、`emit`、`slot` 时，分析调用方影响。

### 数据安全

- 不在代码中硬编码密钥、token、密码等敏感信息。
- 不随意修改生产环境变量或正式环境配置。
- 不移除已有错误处理和异常捕获逻辑。

### 兼容性

- 可见文案必须兼顾国际化。
- 样式修改要考虑 RTL。
- 不引入项目当前不支持的新语法或运行时能力。

## 沟通与回复

AI 应始终使用中文回复用户。完成修改后，给出简要总结，说明改了什么、为什么这么改、如何验证。

不确定的地方要明确标注“需确认”，不要把猜测写成事实。修改前必须先阅读目标文件上下文，了解当前文件所属子项目和业务模块。

## Git 与发布

- 未经用户要求，不主动执行 `git commit` 或 `git push`。
- Qmai 国际仓库需求开发默认从最新 `pre` 分支切出 `feature/*` 分支。
- 禁止直接在 `pre` 分支上修改需求代码。
- 需求分支命名优先使用 `feature/YYYYMMDD-项目需求文档id`。
- 如果无法确定项目需求文档 id，先从需求文档或 Feishu 链接中提取；仍无法确定时，回退使用 `feature/YYYYMMDD-{number}`。
- 发布流程只允许 `pre`；禁止 `gray`、`staging`、`pro`、`prod`、`production`、`tag` 或 `master` / `main` 发布。

例外：当用户明确要求发布或部署 `vue-kylin-international` 组件包时，按 `vue-kylin-international` playbook 的版本升级、`master`、tag、push 流程执行。这不属于普通国际仓库 OPMS `pre` 发布。

## Feishu 与接口文档

Feishu 链接读取链路以本节为唯一事实源。用户输入出现 Feishu 链接时，第一步按 `MCP -> lark-cli -> Codex Chrome 插件` 读取内容，不要反复询问是否需要查看；不得把 `lark-cli` 放在 MCP 之前，也不得把自动化工具作为读取兜底。

| 链接类型 | 读取方式 |
| --- | --- |
| Feishu Project / story / bug / task | 优先使用飞书项目 MCP，例如 `FeishuProjectMcp`；当前会话未暴露对应 MCP 工具但本机配置可用时，先用 shell 手动调用对应 MCP 包，例如 `@lark-project/mcp`；再尝试 `lark-cli` 或其他 CLI；最后使用 Codex Chrome 插件 |
| Feishu wiki / docx / docs | 优先使用飞书文档 / Wiki MCP，例如 `FeishuDocsMcp`；MCP 无法读取时再尝试 `lark-cli wiki/docs/drive`；最后使用 Codex Chrome 插件 |

只有用户明确要求稍后提醒、持续跟进或定时监控时，才进入自动化流程。

MCP、CLI、Codex Chrome 插件都失败，或链接失效、权限不足、内容仍无法读取时，再向用户说明阻塞原因。

如果用户提供接口文档，且接口路径包含 `/sellers`，默认按 `/assistant` 进行接口定位、联调和代码映射，除非用户明确要求保持原样。
