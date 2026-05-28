---
title: "Qmai 项目自动识别"
description: "Qmai Agent Center 如何在不依赖本机路径的情况下识别 Qmai / Qimai 前端项目。"
summary: "通过 package.json、依赖、构建脚本、Vue2 和 qiankun 信号识别项目类型。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 30
tags: ["Qmai Agent Center", "项目识别", "前端工程"]
categories: ["AI 工程化"]
---

本页整理自 `agent-center/rules/project-detection.mdc`。它的核心原则是：不要假设所有子应用都在同一个父目录，也不要要求使用者维护仓库路径映射。

AI 进入任意项目后，应先基于当前项目自身信息判断是否启用 Qmai / Qimai 规则。

## 识别当前项目根目录

进入任意项目后，先确定 `<current-repo-root>`：

- 如果当前目录或上级目录存在 `package.json`，以最近的 `package.json` 所在目录作为项目根目录。
- 如果当前工作区包含多个项目，先根据用户给出的文件、路由、需求关键词定位最相关的 `package.json`。
- 如果无法确定项目根目录，先向用户说明缺少项目上下文。

## Qmai 项目信号

读取 `<current-repo-root>/package.json`。命中以下任意强信号时，按 Qmai 项目处理：

- `scripts` 中出现 `qmai build`、`qmai format`、`qmai postinstall`、`qmai squash`、`qmai deploy` 等命令。
- `dependencies` 或 `devDependencies` 中出现 `@qmai/*`。
- 出现 `eslint-plugin-qmai`。
- 依赖别名中出现 `npm:@qmai/*`。
- 包名或目录名出现 `qmai`、`qimai`，但该项只作为辅助信号。

不要只依赖 `package.json.name` 判断，因为部分 Qmai 项目名称可能是 `org-vue`、`goods-vue`、`console-vue` 等。

## Vue2 后台项目信号

在 Qmai 项目信号基础上，命中以下信号时，按 Qmai Vue2 后台项目处理：

- `vue` 主版本为 `2`。
- 存在 `vue-template-compiler`。
- 存在 `element-ui`。
- 存在 `@qmai/vue-kylin`、`vue-kylin`、`@qmai/qmai-ui` 或 `qmai-ui`。
- 目录名命中 `*-vue` 或 `*-vue-international`，但该项只作为辅助信号。

## qiankun 相关信号

### 基座项目

常见信号包括：

- 依赖中存在 `qiankun`。
- 项目职责涉及登录、菜单、头部、容器布局、微应用注册、跨应用路由。
- 常见仓库名是 `console-vue-international`，但不要只依赖名称。

### 子应用

常见信号包括：

- 源码中存在 qiankun 生命周期、微应用挂载、子应用路由 base、全局状态同步、路由参数注入等逻辑。
- `scripts` 或构建配置中出现 `rsbuild dev`、`qmai build vue`、`qmai build vue-cli-service` 等现行构建入口。
- 常见目录名是 `*-vue-international`。

不要把 `vite-plugin-qiankun` 或 `vite.config.*` 作为当前开发环境的优先判断依据。国际前端现行开发和验证默认走 rsbuild / qmai 构建链路；Vite 相关配置若仍存在，按历史遗留或兼容配置处理。

## 跨仓上下文

项目自动识别只要求读取当前项目。需要跨仓时遵循：

| 场景 | 需要判断的仓库 |
| --- | --- |
| 菜单、登录、容器布局、微前端挂载、跨应用路由 | `console-vue-international` |
| 共享组件、样式一致性、组件 API、`@qmai/vue-kylin` 行为 | `vue-kylin-international` |
| 公共业务页、`/commonCenter/`、打印模板 | `common-vue-international` |

如果当前工作区无法访问相关仓库，不要猜测路径。先基于当前仓库完成可验证分析，并明确说明还需要哪个仓库上下文。

## 未命中时

如果当前项目未命中 Qmai 信号，不要强行套用 Qmai 规则。应按当前项目自身 README、AGENTS、Cursor rules、package scripts 和代码风格处理。
