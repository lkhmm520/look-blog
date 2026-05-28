---
title: "Qmai 国际前端项目接入速查"
description: "Qmai 国际前端项目的仓库分工、目录结构、接口约定、样式入口和组件库依赖关系。"
summary: "快速理解 Qmai 国际前端项目结构：基座、业务子应用、公共业务页、共享组件库、打印运行时与装修平台分别看哪里。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 55
tags: ["Qmai Agent Center", "项目结构", "组件库", "前端工程"]
categories: ["AI 工程化", "前端工程"]
series: ["Qmai Agent Center"]
series_order: 6
---

本页整理自 `agent-center/playbooks/PROJECT_ONBOARDING.md`。它面向需要快速理解公司国际前端项目的人和 AI，回答三个最常见的接入问题：

- 目录结构与接口分工怎么判断
- qiankun 项目的 Sass、主题变量和 UI 约束在哪里
- 共享组件库、Element UI、qmai-ui、vue-kylin 这些组件依赖怎么读

使用前仍应先理解：

1. [Qmai 全局约束]({{< relref "/agent-center/global-constraints/" >}})
2. [Qmai 项目自动识别]({{< relref "/agent-center/project-detection/" >}})
3. [Qmai 工作流路由]({{< relref "/agent-center/workflow-router/" >}})

## 整体分工

```text
进入任意 Qmai 国际前端仓库
  └─ 读取 package.json，识别项目角色
      ├─ console-vue-international
      │   └─ qiankun 基座：登录 / 菜单 / 头部 / 容器布局 / 微应用注册 / 全局状态 / 主题注入
      ├─ *-vue-international
      │   └─ 业务子应用：业务页面 / 路由 / API / i18n / qiankun mount 与 unmount
      ├─ common-vue-international
      │   └─ 公共业务页：/commonCenter/ 公共能力 / 打印模板 / 打印内容布局
      ├─ vue-kylin-international
      │   └─ 共享业务组件库：选择器 / 表格 / 上传 / 业务弹窗 / 组件发布
      ├─ opeartion
      │   └─ 打印运行时：printJs / 打印桥接 / 本地打印逻辑
      ├─ design-vue-3-international
      │   └─ 装修平台：点餐小程序 / SOK / KDS / 智慧屏广告 / 叫号屏装修
      └─ design-v0-international
          └─ 装修平台组件库：被 design-vue-3-international 依赖
```

## 仓库角色判断

不要只靠目录名猜职责。进入项目后优先读当前仓库自己的 `package.json`、`src/main.js`、`src/router/`、`src/api/` 和 qiankun 生命周期代码。

| 仓库类型 | 识别特征 | 主要职责 |
| --- | --- | --- |
| `console-vue-international` | 依赖 `qiankun`，存在 `src/micro-app/` | 基座、登录、菜单、头部、容器、微应用注册、全局状态、主题变量注入 |
| 普通 `*-vue-international` | 依赖 Vue 2、`vue-router`、`vue-kylin`，导出 `bootstrap/mount/unmount` | 业务页面、业务路由、业务 API、子应用内状态、i18n |
| `common-vue-international` | 路由常落在 `/commonCenter/` | 公共业务能力页、公共打印模板、打印内容布局 |
| `vue-kylin-international` | 包名 `@qmai/vue-kylin-international`，入口 `src/index.js` | 共享业务组件库和组件包发布 |
| `opeartion` | 远程为 `git@git.zmcms.cn:framework/opeartion.git`；用户口头提到 `operan` 时通常也指这里 | 打印运行时、打印桥接、本地 `printJs` 调试 |
| `design-vue-3-international` | Vue 3 装修平台，常被口头称为 `design-v3-international` | 点餐小程序、SOK、KDS、智慧屏广告和叫号屏等装修能力 |
| `design-v0-international` | 包名 `@qmai/design-v0-international` | 装修平台组件库，被 `design-vue-3-international` 依赖 |

## 普通业务子应用目录

以 `goods-vue-international` 为典型样本，普通业务子应用一般长这样：

```text
src/
  main.js                 子应用初始化、依赖注册、qiankun 生命周期、基座 props 接入
  router/                 子应用路由定义、路由 meta、页面入口
  views/                  业务页面，按业务域拆目录
  components/             当前子应用内复用组件
  api/                    当前子应用内接口封装
  store/                  当前子应用 Vuex 状态
  lang/                   当前子应用 i18n 文案
  styles/
    global.scss           当前子应用全局样式入口
    utils.scss            Sass 工具函数、mixin 或局部工具样式
  utils/
    request.*             请求封装、baseURL、拦截器、错误处理
```

业务子应用通常在 `mount(props)` 中接收基座透传的 `brand`、`permissions`、`lang`、`onGlobalStateChange`、`setGlobalState` 等数据。

涉及跨应用状态、语言、主题或权限时，不要只看当前页面，需要同步看基座的 `src/micro-app/config.js` 和 `src/micro-app/state.js`。

## 接口分工

```text
页面或组件
  ├─ 当前仓库 src/api/
  │   └─ 当前仓库 request 封装
  │       └─ 后端服务
  └─ vue-kylin 共享组件
      └─ vue-kylin-international/src/vue-kylin/api/
          └─ vue-kylin request 封装
              └─ 后端服务
```

接口定位原则：

- 页面私有接口优先放当前业务仓库 `src/api/`。
- 共享组件内部必需的接口放 `vue-kylin-international/src/vue-kylin/api/`，不要在多个业务仓库复制。
- 请求拦截、错误提示、登录态、品牌态、组织态等横切逻辑先找当前仓库 request 封装。
- 如果 API 文档路径包含 `/sellers`，按全局规则先规范到 `/assistant` 再做代码映射，除非用户明确说不要转换。
- 修改接口入参、响应结构或共享请求封装时，必须分析调用链和下游仓库影响。

## 样式入口

### qiankun 基座样式

`console-vue-international` 是 qiankun 基座，关键入口：

| 文件 | 用途 |
| --- | --- |
| `src/main.js` | 引入 `src/styles/global.scss`，注册 `qmai-ui`、`vue-kylin`、`qm-form` |
| `src/styles/global.scss` | 基座全局 Sass 样式入口 |
| `src/styles/global.css` / `src/styles/global.min.css` | 全局样式构建产物或兼容文件 |
| `src/styles/Roboto/index.css` | 字体样式入口 |
| `src/utils/theme.js` | 根据品牌色生成 CSS 变量并写入 `:root` |
| `src/store/modules/userConfig.js` | 切换明暗主题、品牌色并同步 qiankun 全局状态 |
| `src/micro-app/state.js` | 通过 qiankun `initGlobalState` 向子应用同步 `theme` 等状态 |

主题变量重点看 `src/utils/theme.js`。它会生成并写入：

- `--colorPrimary`
- `--colorPrimaryBg`
- `--colorPrimaryHover`
- `--colorPrimaryActive`
- `--colorPrimaryText`
- `--color-primary-light-1` 到 `--color-primary-light-9`
- `--colorText`
- `--colorBgContainer`
- `--colorBorder`

### 业务子应用 Sass

普通 `*-vue-international` 子应用通常在 `src/main.js` 引入：

```js
import '@/styles/global.scss'
```

典型文件：

| 文件 | 用途 |
| --- | --- |
| `src/styles/global.scss` | 子应用全局样式入口 |
| `src/styles/utils.scss` | 子应用 Sass 工具，业务组件里常通过 `@import '@/styles/utils'` 使用 |
| 单文件组件 `<style lang="scss" scoped>` | 页面或组件局部样式 |

样式改动归属判断：

| 问题类型 | 优先修改位置 |
| --- | --- |
| 菜单、头部、壳层、微应用容器、全局主题变量 | `console-vue-international` |
| 具体业务页面布局、局部表格、局部弹窗 | 当前业务子应用 |
| 多个仓库重复出现的共享组件视觉或行为 | `vue-kylin-international` |
| 公共业务页或打印模板视觉 | `common-vue-international` |
| 打印运行时、打印桥接、本地 printJs 调试 | `opeartion` 打印运行时仓库 |

### UI 样式约束

AI 做 UI 或样式相关任务时，必须先读 [Qmai International 后台设计技术规范]({{< relref "/agent-center/international-design-technical-spec/" >}})。

核心约束：

- 优先复用现有 token，不随意新增全局变量。
- 新增页面默认跟随 `var(--color-primary)` / `var(--colorPrimary)` / `var(--theme)` 等既有主题变量。
- 不要在业务页面大量使用局部 `::v-deep` 覆盖 Element UI 主题色，除非是明确组件级修复。
- 可见文案必须走 i18n。
- 布局要考虑长文本和 RTL，不要依赖中文短文案固定宽度。
- 共享组件问题优先评估 `vue-kylin-international`，不要在多个业务仓库复制补丁。

## 组件库与依赖入口

### vue-kylin 组件库

当前国际组件库仓库：

```text
https://git.zmcms.cn/international/frontend/vue-kylin-international.git
```

关键事实：

| 项 | 值 |
| --- | --- |
| 仓库别名 | `vue-kylin-international` |
| 仓库 package 名 | `@qmai/vue-kylin-international` |
| 消费侧常见引入名 | `vue-kylin`、`@qmai/vue-kylin` |
| 当前入口 | `src/index.js` |
| 组件主目录 | `src/vue-kylin/packages/` |
| 共享内部组件 | `src/vue-kylin/components/` |
| 组件库接口 | `src/vue-kylin/api/` |
| 语言包 | `src/vue-kylin/locale/` |
| 开发与验证入口 | 以 `package.json` scripts 为准，现行公司项目默认走 `rsbuild` / `qmai build` 链路 |
| UMD 全局名 | `VueKylin` |

如果组件库历史配置中存在 `vite.config.*`，只按遗留配置读取，不作为开发环境优先入口。

### 业务项目里的组件依赖

典型业务子应用常见依赖：

| 依赖 | 角色 |
| --- | --- |
| `vue-kylin` | Qmai Vue2 共享业务组件 |
| `@qmai/vue-kylin` | 基座中使用的组件库包名 |
| `element-ui` | Element UI 基础组件，常见版本 `^2.15.14` |
| `@qmai/qmai-ui` / `qmai-ui` | Qmai 基于 Element 体系的 UI 包，常通过 `qmai-ui: npm:@qmai/qmai-ui` alias 引入 |
| `qm-form` | Qmai 表单能力 |
| `vue-i18n` / `vue-i18n-bridge` | 国际化 |

基座 `console-vue-international` 还会在 `src/micro-app/init.js` 中按环境替换微应用模板里的组件库 CDN：

```text
https://rscdn.qmai.com/vue-kylin-international/v${VUE_APP_VUE_KYLIN_VERSION}/index.js
https://rscdn.qmai.cn/npm-packages/qmai-ui/v${VUE_APP_QMAI_UI_VERSION}/index.js
```

因此部署 `vue-kylin-international` 后，需要同步检查 `console-vue-international` 的环境变量 `VUE_APP_VUE_KYLIN_VERSION` 是否等于组件库版本号。

### 装修平台与 design-v0 组件库

| 项 | 说明 |
| --- | --- |
| `design-vue-3-international` | 装修平台主工程，面向点餐小程序、SOK、KDS、智慧屏广告、智慧屏叫号屏等场景 |
| `design-v0-international` | 装修平台组件库，包名 `@qmai/design-v0-international` |
| 依赖关系 | `design-vue-3-international` 通过 `@qmai/design-v0-international` 复用装修组件能力 |
| 技术栈 | `design-vue-3-international` 为 Vue 3 项目，常见 UI 依赖包括 Element Plus |

处理装修平台任务时，先判断问题属于主工程编排、业务场景配置，还是属于 `design-v0` 组件能力。组件能力问题不要只在主工程局部绕开，应同步评估 `design-v0-international`。

## 快速定位命令

进入任意项目后，可以用下面的搜索快速确认样式和组件入口：

```bash
rg -n "vue-kylin|@qmai/vue-kylin|element-ui|qmai-ui|@qmai/qmai-ui|qm-form|global.scss|colorPrimary|--color-primary|qiankun|registerMicroApps|VUE_APP_VUE_KYLIN_VERSION" package.json src .env*
```

如果 shell 对 `.env*` 没有匹配导致报错，去掉 `.env*` 后再查：

```bash
rg -n "vue-kylin|@qmai/vue-kylin|element-ui|qmai-ui|@qmai/qmai-ui|qm-form|global.scss|colorPrimary|--color-primary|qiankun|registerMicroApps|VUE_APP_VUE_KYLIN_VERSION" package.json src
```

## 接入新任务的最小读取顺序

```text
收到任务
  └─ 读当前仓库 package.json
      └─ 按 project-detection 判断仓库类型
          └─ 读 workflow-router 选择最小 playbook
              ├─ 涉及 UI / 样式 / 组件
              │   └─ 读本页，再读后台设计技术规范
              └─ 不涉及 UI / 样式 / 组件
                  └─ 只读命中的业务 playbook
```

接入原则：

- 先判断职责归属，再改代码。
- 能在当前子应用闭环验证的，不扩大到多仓。
- 多仓或共享组件改动必须说明下游影响。
- 不在业务仓库复制共享组件补丁。
- 不为局部页面改动引入新依赖、新样式体系或新目录范式。
