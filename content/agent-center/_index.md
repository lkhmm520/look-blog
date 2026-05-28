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

`Qmai Agent Center` 规范库是团队 AI 前端工作流的文档入口。

这里不按源码目录堆文档，而是按“先理解规则、再识别项目、再选择工作流、最后落到交付和 UI”的顺序组织。想从头阅读可以按下面顺序走；想快速查某个问题，也可以直接跳到对应文档。

专题页：[Qmai Agent Center 专题]({{< relref "/series/qmai-agent-center/" >}})

## 先看哪里

| 我想了解 | 先看 |
| --- | --- |
| 这套东西是什么 | [Qmai Agent Center：前端 AI 工作流规则中心]({{< relref "/ai/qmai-agent-center-introduction/" >}}) |
| AI 做事有哪些底线 | [Qmai 全局约束]({{< relref "/agent-center/global-constraints/" >}}) |
| 当前仓库是不是 Qmai 项目 | [Qmai 项目自动识别]({{< relref "/agent-center/project-detection/" >}}) |
| 一个任务应该进入哪个 playbook | [Qmai 工作流路由]({{< relref "/agent-center/workflow-router/" >}}) |
| 项目目录结构、样式入口、组件库在哪里 | [Qmai 国际前端项目接入速查]({{< relref "/agent-center/project-onboarding/" >}}) |
| 有哪些 playbook，各自管什么 | [Qmai Playbook 索引]({{< relref "/agent-center/playbook-index/" >}}) |
| 需求从分析到交付怎么跑 | [Qmai 国际化交付工作流]({{< relref "/agent-center/international-delivery-workflow/" >}}) |
| 后台页面、组件、主题应该长什么样 | [Qmai International 后台设计技术规范]({{< relref "/agent-center/international-design-technical-spec/" >}}) |

## 阅读顺序

如果只是了解整体机制，建议按下面顺序阅读：

1. 先看 [全局约束]({{< relref "/agent-center/global-constraints/" >}})，理解 AI 做事的底线。
2. 再看 [项目自动识别]({{< relref "/agent-center/project-detection/" >}}) 和 [工作流路由]({{< relref "/agent-center/workflow-router/" >}})，理解 AI 如何进入正确上下文。
3. 想看项目目录结构、样式入口或组件库关系时，进入 [项目结构与组件库接入速查]({{< relref "/agent-center/project-onboarding/" >}})。
4. 需要做需求交付时，进入 [国际化交付工作流]({{< relref "/agent-center/international-delivery-workflow/" >}})。
5. 涉及页面、组件、样式、主题时，进入 [后台 UI 设计技术规范]({{< relref "/agent-center/international-design-technical-spec/" >}})。

## 文档分组

| 分组 | 文档 |
| --- | --- |
| 基础规则 | [全局约束]({{< relref "/agent-center/global-constraints/" >}})、[项目自动识别]({{< relref "/agent-center/project-detection/" >}})、[工作流路由]({{< relref "/agent-center/workflow-router/" >}}) |
| 项目接入 | [项目接入速查]({{< relref "/agent-center/project-onboarding/" >}})、[Playbook 索引]({{< relref "/agent-center/playbook-index/" >}}) |
| 执行交付 | [国际化交付工作流]({{< relref "/agent-center/international-delivery-workflow/" >}}) |
| UI 规范 | [后台设计技术规范]({{< relref "/agent-center/international-design-technical-spec/" >}}) |

## 与源仓库的关系

本规范库来自 `agent-center` 中的核心 Markdown / MDC 文档，博客侧只做阅读组织和视觉化补充。规则发生变化时，应以 `agent-center` 源文档为维护入口，再同步更新这里的公开说明。
