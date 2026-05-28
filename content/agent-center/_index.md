---
title: "Qmai Agent Center 规范库"
description: "Qmai Agent Center 的规则、路由、playbook 与后台 UI 规范入口。"
summary: "这里收纳 Qmai Agent Center 的核心规范文档，方便在博客中直接跳转阅读。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 10
cascade:
  showDate: false
  showAuthor: false
  showReadingTime: false
  showWordCount: false
  showTableOfContents: true
---

`Qmai Agent Center` 的规范库，用来把规则中心中的核心 Markdown 文档整理成可直接打开、可分享、可检索的博客页面。

这些页面不是替代 `agent-center` 源仓库，而是面向团队阅读和传达的文档入口：介绍 AI 在企迈国际化前端项目中应该如何识别项目、选择工作流、遵守全局约束，以及如何按统一的后台 UI 规范落地页面。

## 规范入口

| 文档 | 适用场景 |
| --- | --- |
| [全局约束]({{< relref "/agent-center/global-constraints/" >}}) | 修改流程、最小闭环、安全、Git、Feishu 和接口约束 |
| [项目自动识别]({{< relref "/agent-center/project-detection/" >}}) | 判断当前项目是否应启用 Qmai / Qimai 规则 |
| [工作流路由]({{< relref "/agent-center/workflow-router/" >}}) | 根据任务类型选择最小匹配 playbook |
| [Playbook 索引]({{< relref "/agent-center/playbook-index/" >}}) | 查看所有 playbook 的职责边界和仓库角色 |
| [国际化交付工作流]({{< relref "/agent-center/international-delivery-workflow/" >}}) | 从需求分析到实现、验证、交付的完整闭环 |
| [后台 UI 设计技术规范]({{< relref "/agent-center/international-design-technical-spec/" >}}) | 页面骨架、设计 token、组件规则、UI 样式模板 |

## 阅读顺序

如果只是了解整体机制，建议按下面顺序阅读：

1. 先看 [全局约束]({{< relref "/agent-center/global-constraints/" >}})，理解 AI 做事的底线。
2. 再看 [项目自动识别]({{< relref "/agent-center/project-detection/" >}}) 和 [工作流路由]({{< relref "/agent-center/workflow-router/" >}})，理解 AI 如何进入正确上下文。
3. 需要做需求交付时，进入 [国际化交付工作流]({{< relref "/agent-center/international-delivery-workflow/" >}})。
4. 涉及页面、组件、样式、主题时，进入 [后台 UI 设计技术规范]({{< relref "/agent-center/international-design-technical-spec/" >}})。

## 与源仓库的关系

本规范库来自 `agent-center` 中的核心 Markdown / MDC 文档，博客侧只做阅读组织和视觉化补充。规则发生变化时，应以 `agent-center` 源文档为维护入口，再同步更新这里的公开说明。
