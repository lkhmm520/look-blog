---
title: "Qmai 国际化交付工作流"
description: "从需求分析到实现、验证和交付的 Qmai 国际化前端 AI 工作流。"
summary: "把需求拆成可验证闭环，按仓库归属、分支、实现、验证和交付顺序推进。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 60
tags: ["Qmai Agent Center", "交付流程", "前端工程"]
categories: ["AI 工程化"]
series: ["Qmai Agent Center"]
series_order: 7
---

本页整理自 `agent-center/playbooks/international-delivery-workflow/PLAYBOOK.md`。它适用于从需求分析、实现、验证到交付说明的完整链路。

如果任务只是单点修改，应优先选择更小的 playbook；只有当任务覆盖多个环节时，才进入完整交付工作流。

## 适用范围

- 需求文档已经明确，需要落到国际化前端仓库实现。
- 需要同时完成分析、代码修改、验证和交付说明。
- 需求可能涉及多个仓库、共享组件、公共业务页、qiankun 基座或打印能力。
- 用户希望 AI 按团队研发流程推进，而不只是给出一段代码。

## 交付原则

### 先识别，再实现

开始前先判断当前项目类型、业务归属和可能涉及的上下游仓库。

如果任务涉及菜单、容器、登录、路由、微前端挂载，要判断是否需要 `console-vue-international`。如果涉及共享组件或样式一致性，要判断是否需要 `vue-kylin-international`。如果涉及公共业务页或打印模板，要判断是否需要 `common-vue-international`。

### 先分支，再改代码

Qmai 国际仓库需求开发默认从最新 `pre` 切出 `feature/*` 分支。不要直接在 `pre` 上改需求代码。

分支命名优先使用：

```text
feature/YYYYMMDD-项目需求文档id
```

如果没有可靠的需求文档 id，先从 Feishu 链接或需求文档中提取；仍无法确定时，再使用当天数字序号。

### 保持最小闭环

每个实现阶段都要尽量形成可独立验证的小闭环。不要把需求拆成大量半成品路径，也不要用大重构掩盖具体业务变化。

## 推荐流程

### 1. 读取需求与上下文

- 如果用户提供 Feishu 链接，先按 [Qmai 全局约束]({{< relref "/agent-center/global-constraints/" >}}) 的统一链路读取内容；本 playbook 不重复维护 fallback 顺序。
- 如果只有需求文案，先提取目标页面、目标仓库、接口、角色、状态、异常路径。
- 如果目标仓库不明确，先进入 `international-demand-parser`。

### 2. 判断仓库归属

按照 [项目自动识别]({{< relref "/agent-center/project-detection/" >}}) 与 [工作流路由]({{< relref "/agent-center/workflow-router/" >}}) 确定最小落点。

需要特别关注：

- 当前是否为 Qmai Vue2 后台子应用。
- 是否涉及 qiankun 基座。
- 是否涉及共享组件库。
- 是否涉及公共业务页或打印模板。
- 是否涉及 UI 规范、i18n、RTL 或跨语言长文本。

### 3. 制定修改方案

动手前先给出简要方案：

- 修改目标
- 影响范围
- 涉及文件
- 实现步骤
- 验证方式

多文件或多模块改动必须列出文件清单。

### 4. 实现

实现时遵守：

- 保持现有代码风格。
- 不引入新依赖。
- 不做无关重构。
- 可见文案走 i18n。
- 样式兼顾 RTL 和长文本。
- 接口路径涉及 `/sellers` 时，默认按 `/assistant` 做代码映射。
- `sellerId` 默认不由前端重复传递。

### 5. 验证

按改动范围选择最小验证闭环：

| 改动范围 | 验证方式 |
| --- | --- |
| 文档或规则 | 检查 Markdown 链接、目录入口和引用路径 |
| 单页面逻辑 | 本地路由自测、关键状态覆盖 |
| 样式或 UI | 截图或浏览器检查，覆盖长文本、空态、加载、错误 |
| i18n | 检查翻译 key 和至少一种非中文语言显示 |
| RTL 敏感布局 | 构造 RTL 或逻辑方向样式验证 |
| 共享组件 | 构建组件库并检查至少一个下游调用路径 |
| 子应用页面 | 构建项目，必要时通过 qiankun 基座路由验证 |

### 6. 交付说明

完成后用中文说明：

- 改了什么
- 为什么这么改
- 如何验证
- 还存在哪些需确认事项

不要在用户未要求时执行 `git commit` 或 `git push`。也不要触发 `gray`、`staging`、`pro`、`prod`、tag 或 `main/master` 发布流程。

## 常见分流

| 场景 | 分流 |
| --- | --- |
| 需求文档含糊，目标仓库不明确 | `international-demand-parser` |
| 新增普通后台页面 | `international-vue-subapp` + 必要时 `international-design-technical-spec` |
| 菜单、登录、容器、微前端挂载 | `console-vue-international` |
| 公共业务页、打印模板内容 | `common-vue-international` |
| 打印运行逻辑、本地打印桥接 | `print-component` |
| 共享组件行为或样式 | `vue-kylin-international` |
| OPMS 发布排障 | `shared-opms-release` |

## 交付自查

1. 是否已经明确需求目标与验收标准？
2. 是否确认当前仓库和可能的跨仓影响？
3. 是否已经按规则准备分支？
4. 是否只修改了与目标相关的文件？
5. 是否覆盖空、错、加载、禁用、保存中等关键状态？
6. 是否兼顾 i18n、RTL 和长文本？
7. 是否执行了最小验证闭环？
8. 是否在交付说明中标注需确认事项？
