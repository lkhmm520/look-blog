---
title: "Qmai 工作流路由"
description: "Qmai Agent Center 根据任务类型选择最小匹配 playbook 的路由规则。"
summary: "从项目识别进入最小 playbook，避免 AI 只靠临场猜测。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 40
tags: ["Qmai Agent Center", "Playbook", "工作流路由"]
categories: ["AI 工程化"]
---

本页整理自 `agent-center/rules/workflow-router.mdc`。它是 Qmai Agent Center 的唯一权威路由源，用来决定当前任务应该进入哪个最小 playbook。

使用本规则前，应先读取：

1. [Qmai 全局约束]({{< relref "/agent-center/global-constraints/" >}})
2. [Qmai 项目自动识别]({{< relref "/agent-center/project-detection/" >}})
3. [Qmai Playbook 索引]({{< relref "/agent-center/playbook-index/" >}})

## 路由原则

- 先基于当前项目自身特征识别项目类型，再选择最小匹配 playbook。
- 路由规则只维护在 `rules/workflow-router.mdc`。
- `playbooks/INDEX.md` 只承担索引与说明职责，不重复维护完整路由规则。
- 不假设所有仓库位于同一个父目录。
- 不要求使用者维护仓库路径映射。
- 不把个人本机路径写入规则或交付说明。
- 当前工作区不可见的跨仓上下文，只能明确说明缺失，不要猜路径。

## 任务路由表

| 任务类型 | 进入的 playbook |
| --- | --- |
| 飞书链接、需求文案、目标仓库不明确 | `international-demand-parser` |
| 需求分析到实现、验证、交付完整链路 | `international-delivery-workflow` |
| 普通 Qmai Vue2 后台子应用页面、组件、路由、API 改动 | `international-vue-subapp` |
| qiankun 基座、登录、菜单、头部、容器布局、微应用挂载、跨应用状态 | `console-vue-international` |
| 公共业务页、`/commonCenter/`、打印模板、打印内容布局 | `common-vue-international` |
| 打印运行逻辑、打印组件、打印插件、本地打印桥接代码 | `print-component` |
| 共享 UI 组件、样式一致性、`@qmai/vue-kylin` 行为 | `vue-kylin-international` |
| `vue-kylin-international` 版本升级、package 发布、打 tag、推送或部署 | `vue-kylin-international` |
| UI / UX / 页面布局 / 设计 token / 后台页面视觉规范 | `international-design-technical-spec` |
| 项目接入说明、目录结构分工、接口职责图、样式入口、组件库地址与依赖关系梳理 | `PROJECT_ONBOARDING.md` 后再进入最小 playbook |
| `pre` 发布、发布状态检查、OPMS 排障 | `shared-opms-release` |
| 刷新 playbook 与真实仓库历史同步 | `PLAYBOOK_SYNC.md` |

## 路径占位

playbook 中的路径应使用统一占位，而不是写死某个人的本机路径。

| 占位 | 含义 |
| --- | --- |
| `<agent-center-root>` | 当前规则中心目录 |
| `<playbooks-root>` | `<agent-center-root>/playbooks` |
| `<current-repo-root>` | 当前项目根目录 |
| `<workspace-root>` | 当前 AI 工具打开的工作区目录 |

如需引用用户本机路径，只能作为示例，不能作为规则前提。
