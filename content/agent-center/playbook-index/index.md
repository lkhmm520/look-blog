---
title: "Qmai Playbook 索引"
description: "Qmai Agent Center 的 playbook 清单、职责边界与仓库角色说明。"
summary: "列出可用 playbook、适用场景和仓库角色，帮助 AI 进入正确上下文。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 50
tags: ["Qmai Agent Center", "Playbook", "前端工程"]
categories: ["AI 工程化"]
---

本页整理自 `agent-center/playbooks/INDEX.md`。它负责说明有哪些 playbook、分别解决什么问题，以及对应文件位于哪里。

完整路由规则仍以 [Qmai 工作流路由]({{< relref "/agent-center/workflow-router/" >}}) 为准。

## 使用前置

使用 playbook 前，应先读取：

1. [Qmai 全局约束]({{< relref "/agent-center/global-constraints/" >}})
2. [Qmai 项目自动识别]({{< relref "/agent-center/project-detection/" >}})
3. [Qmai 工作流路由]({{< relref "/agent-center/workflow-router/" >}})

## Playbooks

| Playbook | 职责 |
| --- | --- |
| `international-demand-parser` | 输入只有 Feishu 链接、需求文案或工单，且目标仓库不明确时使用 |
| `international-delivery-workflow` | 任务覆盖需求分析、实现、验证和交付完整链路时使用 |
| `international-vue-subapp` | 改动明确落在普通 Qmai Vue2 后台子应用页面、组件、路由或 API 时使用 |
| `console-vue-international` | 涉及 qiankun 基座、菜单、登录、壳层布局、微应用挂载或跨应用状态时使用 |
| `common-vue-international` | 涉及公共业务页、`/commonCenter/`、打印模板或打印内容布局时使用 |
| `print-component` | 只涉及打印运行逻辑、打印桥接、打印插件或本地调试代码时使用 |
| `vue-kylin-international` | 共享 UI 组件、样式一致性、`@qmai/vue-kylin` 行为或组件库发布流程 |
| `international-design-technical-spec` | 后台视觉规范、页面骨架、设计 token 或设计对照预览 |
| `shared-opms-release` | 普通国际仓库 `pre` 发布、状态检查或 OPMS 排障 |
| `PROJECT_ONBOARDING` | 新人或 AI 接入说明、目录结构分工、样式入口、组件库与依赖关系 |
| `PLAYBOOK_SYNC` | playbook 与真实仓库最新默认分支重新对齐 |

## 仓库角色别名

这些名称是仓库角色别名，不代表本机路径。需要跨仓上下文时，先判断当前工作区是否可见；不可见时说明缺少对应仓库上下文。

| 别名 | 职责 |
| --- | --- |
| `console-vue-international` | qiankun 基座、登录、菜单、头部、容器布局、共享路由容器 |
| `common-vue-international` | 公共业务能力页、`/commonCenter/`、打印模板和打印内容布局 |
| `vue-kylin-international` | Qmai Vue2 共享 UI 组件库与组件包版本发布流程 |
| `shared-opms-release` | 共享 OPMS `pre` 发布流程 |
| `print-component` | 打印运行逻辑、打印组件、打印插件、本地打印桥接 |

## 常见业务子应用

| 仓库 | 业务域 |
| --- | --- |
| `org-vue-international` | 组织、企业、员工、账号、门店、语言、权限 |
| `coupon-vue-international` | 优惠券、卡券、活动、券分析、券配置 |
| `goods-vue-international` | 商品、菜单、SKU、商品资料 |
| `member-vue-international` | 会员、CRM、忠诚度 |
| `marketing-vue-international` | 营销活动与营销能力页 |
| `payment-vue-international` | 支付能力页 |
| `operating-vue-international` | 运营侧业务页 |
| `connector-vue-international`、`catering-vue-international`、`scm-vue-international`、`std-vue-international`、`bi-vue-international`、`message-vue-international` | 按业务域、路由前缀和实际页面路径判断 |

## 维护规则

- 刷新项目 playbook 时，以目标仓库最新默认分支作为事实来源，而不是当前 feature 分支。
- 每次刷新后记录并推进 `skill-baselines.json` 中对应基线。
- 路由逻辑变化时，先更新 `rules/workflow-router.mdc`，再同步必要的索引说明。
