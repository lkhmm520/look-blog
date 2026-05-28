---
title: "Codex CLI 使用指南"
date: 2026-01-29T14:00:00+08:00
# ...其他配置

# 【核心字段】专门用于列表页显示的摘要
summary: "这是一个基于 Codex 的命令行工具，能让你直接在终端与 AI 对话，无需切换浏览器。本文介绍了它的安装、配置及常用命令。"

# (可选) description 通常用于 SEO 的 meta标签，有些主题也会混用
description: "Codex CLI 完整技术文档"

cover:
  image: "cover.png"     # 直接写文件名
  alt: "codex-cli" 
  # caption: "图片底部的说明文字"
  relative: true         # 【关键】告诉主题这是相对路径，去当前文件夹找
# 必须加上这行，标签页才会有东西！
# 注意格式：tags: ["标签1", "标签2"]
tags: ["AI", "CLI", "开发工具"] 

# 可选：如果你想分类
categories: ["技术文档"]
---

# Codex CLI 使用文档

## 一、为什么选择 Codex CLI

### 1.1 对比一览

| 维度     | 传统 AI Chat 对话        | Codex CLI                                                                                                                                     |
| -------- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------- |
| 入口     | 浏览器/网页为主          | 终端统一入口，直接在本地仓库内工作（编辑器无关） [developers.openai.com](https://developers.openai.com/codex/cli?utm_source=openai)           |
| 上下文   | 主要靠手动粘贴/描述      | 可读取仓库文件与变更，任务上下文持续 [developers.openai.com](https://developers.openai.com/codex/cli?utm_source=openai)                       |
| 可信控制 | 缺少命令级审批与执行控制 | 内建审批模式与会话状态，可控制何时执行命令 [developers.openai.com](https://developers.openai.com/codex/cli/reference/?utm_source=openai)      |
| 复用能力 | 以对话模板/复制粘贴为主  | 支持内置 Slash 命令、可扩展 Prompt 与 Skill [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai) |
| 扩展     | 需要手动切换外部工具     | 通过 MCP 接入第三方工具与上下文（如 Figma） [developers.openai.com](https://developers.openai.com/codex/mcp/?utm_source=openai)               |

## 二、快速开始

### 2.1 安装

```bash
npm i -g @openai/codex
```

### 2.2 进入项目并启动

```bash
codex
```

### 2.3 登录

首次运行会提示登录（ChatGPT 账号或 API key）。 [developers.openai.com](https://developers.openai.com/codex/cli?utm_source=openai)
## 三、快捷键指令

### 3.1 常用快捷键

- /：命令列表
- !：Shell 命令
- Ctrl+J：换行
- @：补全文件路径
- Ctrl+G：外部编辑器输入
- Ctrl+T：查看对话记录
- Ctrl+C：退出
- Tab：排队发送消息
- Ctrl+V：粘贴图片（Mac/Windows/Linux 通用）
- Esc Esc：编辑上一条消息

## 四、Slash 命令速查（CLI 内置）

### 4.1 命令列表

在 CLI 输入 `/` 可弹出命令列表并过滤。 [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai)

| 命令        | 用途                                 |
| ----------- | ------------------------------------ |
| /approvals  | 设置审批模式（例如只读/自动/需确认） |
| /compact    | 对话压缩，保留要点                   |
| /diff       | 查看 Git diff（含未跟踪文件）        |
| /exit /quit | 退出 CLI                             |
| /feedback   | 发送诊断与反馈                       |
| /init       | 生成 AGENTS.md 模板                  |
| /logout     | 退出登录                             |
| /mcp        | 查看已配置 MCP 工具                  |
| /mention    | 添加文件/路径到上下文                |
| /model      | 切换模型                             |
| /new        | 开新对话（同目录）                   |
| /review     | 代码/改动审查                        |
| /status     | 查看会话配置与状态                   |
| /undo       | 撤销最近一次变更                     |

## 五、常用命令演示流程

### 5.1 初始化项目指令

```
/init
```

生成 AGENTS.md 模板，作为项目级指令入口。 [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai)

### 5.2 限制或放开权限

```
/approvals
```

选择审批策略（例如只读/自动/需确认）。 [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai)

### 5.3 关注关键文件

这一环节的核心是：把“你正在演示的关键文件”加入上下文，不必拘泥固定路径。以下给出两种常见场景：

- 如果有现成文件（例如纯 HTML 待办演示）：
  ```
  /mention index.html
  ```
- 如果目录暂时还不存在（或准备新建）：
  ```
  /mention README.md
  ```
  然后让 Codex 生成/创建你要演示的目标文件。

将文件加入上下文后，后续指令会自动带上，更方便联动修改。 [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai)

### 5.4 查看改动

```
/diff
```

### 5.5 审查改动

```
/review
```

## 六、示例：用 Codex CLI 生成一个纯 HTML 待办事项页

目标：生成一个可直接打开的静态待办页面（HTML + CSS + 原生 JS，支持本地保存）。

### 6.1 准备项目

```bash
mkdir codex-demo-page
cd codex-demo-page
touch index.html
codex
```

### 6.2 在 Codex CLI 输入

请生成一个纯 HTML 待办事项页面：包含任务输入、列表、完成状态、筛选（全部/未完成/已完成），
数据存 localStorage；要求响应式、排版清晰、配色干净。

### 6.3 检查改动

```
/diff
/review
```

### 6.4 可选本地预览

```bash
python -m http.server 8000
```

## 七、AGENTS.md 使用说明（含完整模板）

### 7.1 作用

为当前仓库提供持续的指令，Codex 会自动读取并遵守。 [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai)

### 7.2 生成方式

```
/init
```

### 7.3 推荐模板（可直接复制）

```
# AGENTS.md

## Repository Guidelines

### 项目结构与模块组织
- 说明主要目录与模块的职责边界
- 指出入口文件与页面组件位置

### 构建与开发命令
- 统一列出安装、开发、构建、预览命令

### 编码风格
- 缩进、命名规范、组件职责边界

### 数据存储与约束
- localStorage key 命名、后端接口、字段约束

### 代码审查重点
- 修改功能时需补充的测试点
- 高风险模块注意事项

### 输出要求（每次改动后需说明）
- 说明改动内容（做了什么）
- 说明影响范围（哪些文件/模块/功能受影响）
- 提供回滚或兜底方案（如何恢复到改动前）
```

## 八、MCP（Model Context Protocol）用法（以 Figma 为例）

### 8.1 作用

让 Codex 接入第三方工具与上下文（如设计稿、浏览器、文档）。 [developers.openai.com](https://developers.openai.com/codex/mcp/?utm_source=openai)

### 8.2 方式 1：CLI 添加 MCP Server

```bash
# 使用 HTTP MCP Server（以 Figma 为例，URL 以实际服务商提供为准）
codex mcp add figma --url https://mcp.figma.com/mcp

# 查看已配置的 MCP 列表
codex mcp list
```

### 8.3 方式 2：配置文件（项目或全局）

```toml
# ~/.codex/config.toml 或 <repo>/.codex/config.toml
[mcp_servers.figma]
url = "https://mcp.figma.com/mcp"
```

### 8.4 CLI 内查看 MCP

```
/mcp
```

## 九、Skills：把常用流程封装成可复用能力

### 9.1 定义与组成

Skill 是一个文件夹，核心是 `SKILL.md`（必需）+ 可选的 `scripts/`、`references/`、`assets/`。 [developers.openai.com](https://developers.openai.com/codex/skills?utm_source=openai)

`SKILL.md` 使用 YAML Front Matter 描述元信息（至少 `name` 与 `description`），正文写具体流程说明。

### 9.2 自动识别与触发方式

Codex 启动时会扫描可用 Skill，只读取 `name` 与 `description` 用于匹配；只有当 Skill 被触发时才加载完整内容与引用。 [developers.openai.com](https://developers.openai.com/codex/skills?utm_source=openai)

触发方式有两种：
- 显式：输入 `/skills` 选择，或在提示中直接写 `$skill-name`
- 隐式：当任务描述与 Skill 的 `description` 匹配时，Codex 自动启用

### 9.3 保存位置（由近到远）

技能的加载位置由 Team Config 定义，Codex 会扫描以下目录： [developers.openai.com](https://developers.openai.com/codex/skills?utm_source=openai)

1) `$CWD/.codex/skills`
2) `$CWD/../.codex/skills`
3) `$REPO_ROOT/.codex/skills`
4) `$CODEX_HOME/skills`（macOS/Linux 默认 `~/.codex/skills`）
5) `/etc/codex/skills`
6) 系统内置 Skills（随 Codex 发布）

说明：若出现同名 Skill，Codex 不会去重，选择器里可能同时出现多个。Codex 也支持扫描到的技能目录为软链接。 [developers.openai.com](https://developers.openai.com/codex/skills?utm_source=openai)

### 9.4 创建与安装（推荐流程）

**方式 1：使用内置 `$skill-creator`**  
在 Codex CLI 中描述你要的能力，Codex 会引导你创建 Skill。 [developers.openai.com](https://developers.openai.com/codex/skills?utm_source=openai)

**方式 2：手动创建**  
在任意有效技能目录下创建文件夹与 `SKILL.md`，并在保存后重启 Codex 生效。

**安装官方示例（experimental）**  
```
$skill-installer install the create-plan skill from the .experimental folder
```
安装完成后重启 Codex 生效。

### 9.5 SKILL.md（完整示例，可直接复制）

```md
---
name: Daily Git Log Report
description: 根据指定目录路径生成日报摘要
---

## 目标
在指定仓库与目录范围内，收集当天 git log 并生成日报摘要。

## 输入要求
在执行前向用户确认以下信息：
1) REPO_PATH：仓库路径（绝对路径）
2) FOCUS_PATH：仅统计的目录或文件路径（相对 REPO_PATH）
3) DATE：日报日期（YYYY-MM-DD；若未提供则使用当天）

## 执行步骤
1) 在 shell 中执行 git log，范围限定为 DATE 当天。
2) 排除 merge commit。
3) 输出清单 + 摘要两段。

## 命令模板
git -C "$REPO_PATH" log \
  --since "$DATE 00:00" \
  --until "$DATE 23:59" \
  --no-merges \
  --date=short \
  --pretty=format:"%h %ad %s (%an)" \
  -- "$FOCUS_PATH"

## 输出格式

### Commit 清单

- 逐条列出：hash + 日期 + 标题 + 作者

### 日报摘要

- 归纳为 3~6 条重点
- 按功能模块或主题归类
```

## 十、类似 “Hooks” 的能力替代方案

### 10.1 说明

官方文档聚焦于 Slash 命令、Skills、MCP，并未描述与“自动触发 Hook”等价的机制。 [developers.openai.com](https://developers.openai.com/codex/cli/slash-commands?utm_source=openai)

可替代方案：
- Git hooks：在 pre-commit / pre-push 中调用 Codex 或固定脚本
- Skills：把流程固化成可分享的能力（比脚本更可读）


### 总结

Codex CLI 是一个终端内的本地编码代理，能在你的仓库里直接执行、编辑与协作。它解决了“上下文零散、工具切换频繁、流程难复用、权限不可控”的痛点，让你在统一入口里完成任务。学习并熟练使用后，可获得更高的执行效率、更稳定的流程复用，以及更可控的团队协作与扩展能力。 [developers.openai.com](https://developers.openai.com/codex/cli?utm_source=openai)
