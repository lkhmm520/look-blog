---
title: "Qmai International 后台设计技术规范"
description: "Qmai 国际化后台页面的视觉、交互、设计 token、组件规则和 UI 样式模板。"
summary: "用于新增或改造 Qmai 国际化后台页面，保持白底工作面、绿色主动作、清晰层级、i18n 与 RTL 兼容。"
date: 2026-05-28T00:00:00+08:00
draft: false
weight: 70
tags: ["Qmai Agent Center", "UI 规范", "Design System", "前端工程"]
categories: ["AI 工程化", "前端工程"]
---

本页整理自 `agent-center/playbooks/international-design-technical-spec/PLAYBOOK.md`，并在文档中直接加入可视化模板，方便团队成员快速理解后台页面、组件和主题应该长什么样。

这份规范用于 Qmai 国际化后台仓库，目标是把“商家后台设计包”的视觉与交互原则，落到现有 Vue 2、qiankun、`@qmai/vue-kylin`、Element UI、i18n、RTL 和多仓库协作流程中。

## 适用范围

适用于以下工作：

- 新增或改造 `*-vue-international` 后台页面。
- 页面布局、视觉层级、表格、筛选区、配置页、编辑器、选择器、工作台类改造。
- 多仓库之间的 UI / 交互一致性收敛。
- `console-vue-international` 壳层、菜单、头部、容器布局相关设计调整。
- `common-vue-international` 公共业务页、打印模板页、配置页相关设计调整。
- `vue-kylin-international` 共享组件的视觉、交互、文案、状态规范调整。

不适用于：

- H5 活动页的营销视觉规范。
- Flutter / App 原生端规范。
- 纯接口、纯数据结构、纯构建链路问题。
- 打印运行时逻辑的代码规范；但打印预览与纸面模板视觉仍可参考本规范。

## 总体定位

Qmai 国际后台是面向连锁餐饮总部、区域运营、门店管理、实施、财务、营销与配置人员的高频业务工作台。

它不是营销官网、数据大屏秀场、通用 SaaS 卡片模板，也不是每个业务仓库各自发明风格的拼盘。

它必须是：

- 专业
- 克制
- 清晰
- 高信任
- 可治理
- 可复用
- 国际化友好

核心情绪目标是：让用户因为界面有秩序，而敢于判断、配置、保存和发布。

## 全局设计原则

### 结构先于装饰

优先用页面骨架、信息顺序、对齐、留白、边框和分组关系解决问题。颜色、阴影、插画、渐变只能作为辅助信号，不能替代结构。

### 层级先于颜色

标题、主信息、辅助信息、操作入口的优先级，先通过尺寸、字重、位置、间距、容器关系表达。颜色只表达动作、状态和风险。

### 绿色是默认动作色

默认主动作、激活态、启用态和正向状态使用绿色家族。除非产品主题或现有组件 token 已明确约束，否则不要把蓝色继续扩展为新页面主动作语言。

### 白底是工作面

业务内容区默认回到白底。壳层可以有冷静浅色背景或轻微渐变，但不能把壳层装饰复制到所有业务卡片。

### 页面先归类，再设计

新增页面必须先判断 archetype，再写结构。默认 archetype：

- 列表页
- 配置页
- 全屏编辑器
- 选择器
- 工作台 / 画布页
- 数据总览页

无法归入以上类型时，才允许提出新骨架。

## UI 样式模板

下面这块模板直接渲染在 Markdown 页面里，用来展示 Qmai 国际化后台的组件主题、页面骨架和视觉密度。它是规范阅读用的可视化样张，不是生产业务页面。

<style>
.qmai-spec-preview {
  --qmai-primary: #00b578;
  --qmai-primary-dark: #009a66;
  --qmai-primary-soft: #e8f8f1;
  --qmai-ink: #101828;
  --qmai-text: #344054;
  --qmai-muted: #667085;
  --qmai-weak: #98a2b3;
  --qmai-border: #d0d5dd;
  --qmai-line: #eaecf0;
  --qmai-surface: #f8fafb;
  --qmai-field: #f5f7f8;
  --qmai-info: #2e90fa;
  --qmai-warning: #f79009;
  --qmai-danger: #d92d20;
  margin: 24px 0;
  padding: 24px;
  border: 1px solid var(--qmai-line);
  border-radius: 18px;
  background: #f6f8fa;
  color: var(--qmai-text);
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", Arial, sans-serif;
}
.qmai-spec-preview * {
  box-sizing: border-box;
}
.qmai-spec-preview h3,
.qmai-spec-preview h4,
.qmai-spec-preview p {
  margin: 0;
}
.qmai-preview-shell {
  overflow: hidden;
  border: 1px solid #dfe5ea;
  border-radius: 14px;
  background: #fff;
  box-shadow: 0 18px 45px rgba(16, 24, 40, 0.08);
}
.qmai-preview-topbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 16px;
  padding: 14px 18px;
  border-bottom: 1px solid var(--qmai-line);
  background: #fff;
}
.qmai-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  font-size: 14px;
  font-weight: 700;
  color: var(--qmai-ink);
}
.qmai-brand-mark {
  width: 28px;
  height: 28px;
  border-radius: 8px;
  background: linear-gradient(135deg, var(--qmai-primary), #38d99d);
  box-shadow: inset 0 0 0 1px rgba(255, 255, 255, 0.5);
}
.qmai-preview-actions {
  display: flex;
  flex-wrap: wrap;
  align-items: center;
  gap: 8px;
}
.qmai-btn {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-height: 34px;
  padding: 0 14px;
  border: 1px solid var(--qmai-border);
  border-radius: 8px;
  background: #fff;
  color: var(--qmai-text);
  font-size: 13px;
  font-weight: 600;
  line-height: 1;
  white-space: nowrap;
}
.qmai-btn-primary {
  border-color: var(--qmai-primary);
  background: var(--qmai-primary);
  color: #fff;
}
.qmai-btn-danger {
  border-color: #fecdca;
  background: #fff;
  color: var(--qmai-danger);
}
.qmai-preview-body {
  display: grid;
  grid-template-columns: 210px minmax(0, 1fr);
  min-height: 520px;
  background: var(--qmai-surface);
}
.qmai-side {
  padding: 16px 12px;
  border-right: 1px solid var(--qmai-line);
  background: #fff;
}
.qmai-nav-item {
  display: flex;
  align-items: center;
  gap: 10px;
  min-height: 38px;
  padding: 0 12px;
  border-radius: 8px;
  color: var(--qmai-muted);
  font-size: 13px;
  font-weight: 600;
}
.qmai-nav-item::before {
  width: 8px;
  height: 8px;
  border-radius: 99px;
  background: #cfd6dd;
  content: "";
}
.qmai-nav-item.is-active {
  background: var(--qmai-primary-soft);
  color: var(--qmai-primary-dark);
}
.qmai-nav-item.is-active::before {
  background: var(--qmai-primary);
}
.qmai-main {
  display: grid;
  gap: 16px;
  padding: 18px;
}
.qmai-page-head {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 18px;
  padding: 18px;
  border: 1px solid var(--qmai-line);
  border-radius: 12px;
  background: #fff;
}
.qmai-page-title {
  font-size: 22px;
  font-weight: 800;
  line-height: 1.25;
  color: var(--qmai-ink);
}
.qmai-page-desc {
  margin-top: 6px;
  max-width: 660px;
  color: var(--qmai-muted);
  font-size: 13px;
  line-height: 1.6;
}
.qmai-panel {
  border: 1px solid var(--qmai-line);
  border-radius: 12px;
  background: #fff;
}
.qmai-panel-head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 12px;
  padding: 14px 16px;
  border-bottom: 1px solid var(--qmai-line);
}
.qmai-panel-title {
  font-size: 15px;
  font-weight: 800;
  color: var(--qmai-ink);
}
.qmai-token-grid {
  display: grid;
  grid-template-columns: repeat(4, minmax(0, 1fr));
  gap: 12px;
  padding: 16px;
}
.qmai-token {
  min-height: 92px;
  padding: 12px;
  border: 1px solid var(--qmai-line);
  border-radius: 10px;
  background: #fff;
}
.qmai-swatch {
  width: 100%;
  height: 28px;
  margin-bottom: 10px;
  border-radius: 7px;
  border: 1px solid rgba(16, 24, 40, 0.08);
}
.qmai-token strong {
  display: block;
  font-size: 12px;
  color: var(--qmai-ink);
}
.qmai-token span {
  display: block;
  margin-top: 4px;
  color: var(--qmai-muted);
  font-size: 12px;
}
.qmai-filter {
  display: grid;
  grid-template-columns: minmax(180px, 1fr) minmax(140px, 180px) auto auto;
  gap: 10px;
  padding: 16px;
  border-bottom: 1px solid var(--qmai-line);
}
.qmai-field {
  display: flex;
  align-items: center;
  min-height: 36px;
  padding: 0 12px;
  border: 1px solid var(--qmai-border);
  border-radius: 8px;
  background: #fff;
  color: var(--qmai-muted);
  font-size: 13px;
}
.qmai-field.is-focus {
  border-color: var(--qmai-primary);
  box-shadow: 0 0 0 3px rgba(0, 181, 120, 0.12);
}
.qmai-table {
  width: 100%;
  border-collapse: collapse;
  font-size: 13px;
}
.qmai-table th,
.qmai-table td {
  padding: 12px 16px;
  border-bottom: 1px solid var(--qmai-line);
  text-align: left;
}
.qmai-table th {
  background: var(--qmai-surface);
  color: var(--qmai-muted);
  font-weight: 700;
}
.qmai-row-title {
  color: var(--qmai-ink);
  font-weight: 700;
}
.qmai-tag {
  display: inline-flex;
  align-items: center;
  min-height: 24px;
  padding: 0 8px;
  border-radius: 999px;
  border: 1px solid transparent;
  font-size: 12px;
  font-weight: 700;
  white-space: nowrap;
}
.qmai-tag-success {
  border-color: #abefc6;
  background: #ecfdf3;
  color: #067647;
}
.qmai-tag-warning {
  border-color: #fedf89;
  background: #fffaeb;
  color: #b54708;
}
.qmai-tag-info {
  border-color: #b2ddff;
  background: #eff8ff;
  color: #175cd3;
}
.qmai-layout-grid {
  display: grid;
  grid-template-columns: minmax(0, 1fr) 280px;
  gap: 16px;
  padding: 16px;
}
.qmai-canvas {
  min-height: 220px;
  padding: 18px;
  border: 1px dashed #cbd5e1;
  border-radius: 12px;
  background:
    linear-gradient(#fff, #fff) padding-box,
    repeating-linear-gradient(90deg, #eef2f6 0, #eef2f6 1px, transparent 1px, transparent 28px);
}
.qmai-receipt {
  width: min(240px, 100%);
  margin: 0 auto;
  padding: 16px;
  border: 1px solid var(--qmai-line);
  border-radius: 10px;
  background: #fff;
  box-shadow: 0 12px 28px rgba(16, 24, 40, 0.08);
}
.qmai-receipt-line {
  height: 10px;
  margin-top: 10px;
  border-radius: 99px;
  background: #e4e7ec;
}
.qmai-config {
  display: grid;
  gap: 10px;
}
.qmai-config-row {
  padding: 12px;
  border: 1px solid var(--qmai-line);
  border-radius: 10px;
  background: #fff;
}
.qmai-config-label {
  margin-bottom: 8px;
  color: var(--qmai-ink);
  font-size: 13px;
  font-weight: 700;
}
.qmai-switch {
  width: 42px;
  height: 24px;
  padding: 3px;
  border-radius: 999px;
  background: var(--qmai-primary);
}
.qmai-switch::before {
  display: block;
  width: 18px;
  height: 18px;
  margin-left: auto;
  border-radius: 999px;
  background: #fff;
  content: "";
}
.qmai-footer-bar {
  display: flex;
  justify-content: flex-end;
  gap: 8px;
  padding: 12px 16px;
  border-top: 1px solid var(--qmai-line);
  background: #fff;
}
@media (max-width: 880px) {
  .qmai-spec-preview {
    padding: 14px;
  }
  .qmai-preview-body,
  .qmai-layout-grid {
    grid-template-columns: 1fr;
  }
  .qmai-side {
    display: none;
  }
  .qmai-page-head,
  .qmai-preview-topbar {
    flex-direction: column;
    align-items: stretch;
  }
  .qmai-token-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .qmai-filter {
    grid-template-columns: 1fr;
  }
  .qmai-table {
    min-width: 620px;
  }
  .qmai-table-wrap {
    overflow-x: auto;
  }
}
@media (max-width: 520px) {
  .qmai-token-grid {
    grid-template-columns: 1fr;
  }
}
</style>

<div class="qmai-spec-preview">
  <div class="qmai-preview-shell">
    <div class="qmai-preview-topbar">
      <div class="qmai-brand"><span class="qmai-brand-mark"></span><span>Qmai Backoffice</span></div>
      <div class="qmai-preview-actions">
        <span class="qmai-btn">取消</span>
        <span class="qmai-btn qmai-btn-primary">保存配置</span>
      </div>
    </div>
    <div class="qmai-preview-body">
      <aside class="qmai-side">
        <div class="qmai-nav-item is-active">规则配置</div>
        <div class="qmai-nav-item">商品范围</div>
        <div class="qmai-nav-item">门店策略</div>
        <div class="qmai-nav-item">发布记录</div>
      </aside>
      <main class="qmai-main">
        <section class="qmai-page-head">
          <div>
            <h3 class="qmai-page-title">订阅转换规则</h3>
            <p class="qmai-page-desc">白底工作面、清晰层级、稳定保存路径；主动作使用绿色，状态和风险用语义色表达。</p>
          </div>
          <div class="qmai-preview-actions">
            <span class="qmai-btn">预览</span>
            <span class="qmai-btn qmai-btn-primary">新建规则</span>
          </div>
        </section>
        <section class="qmai-panel">
          <div class="qmai-panel-head">
            <h4 class="qmai-panel-title">Design Tokens</h4>
            <span class="qmai-tag qmai-tag-success">Primary</span>
          </div>
          <div class="qmai-token-grid">
            <div class="qmai-token"><div class="qmai-swatch" style="background:#00b578"></div><strong>Color Primary</strong><span>#00B578</span></div>
            <div class="qmai-token"><div class="qmai-swatch" style="background:#101828"></div><strong>Ink 900</strong><span>#101828</span></div>
            <div class="qmai-token"><div class="qmai-swatch" style="background:#eaecf0"></div><strong>Border 200</strong><span>#EAECF0</span></div>
            <div class="qmai-token"><div class="qmai-swatch" style="background:#f8fafb"></div><strong>Surface 50</strong><span>#F8FAFB</span></div>
          </div>
        </section>
        <section class="qmai-panel">
          <div class="qmai-panel-head">
            <h4 class="qmai-panel-title">列表页模板</h4>
            <div class="qmai-preview-actions">
              <span class="qmai-tag qmai-tag-info">稳定扫描</span>
              <span class="qmai-tag qmai-tag-warning">风险确认</span>
            </div>
          </div>
          <div class="qmai-filter">
            <div class="qmai-field is-focus">搜索规则名称 / ID</div>
            <div class="qmai-field">状态：全部</div>
            <span class="qmai-btn">重置</span>
            <span class="qmai-btn qmai-btn-primary">查询</span>
          </div>
          <div class="qmai-table-wrap">
            <table class="qmai-table">
              <thead>
                <tr>
                  <th>规则名称</th>
                  <th>适用范围</th>
                  <th>状态</th>
                  <th>最近更新</th>
                  <th>操作</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><span class="qmai-row-title">会员订阅转换</span></td>
                  <td>总部 / 区域门店</td>
                  <td><span class="qmai-tag qmai-tag-success">已启用</span></td>
                  <td>2026-05-28</td>
                  <td>编辑 · 复制 · 更多</td>
                </tr>
                <tr>
                  <td><span class="qmai-row-title">渠道分流策略</span></td>
                  <td>线上渠道</td>
                  <td><span class="qmai-tag qmai-tag-warning">待发布</span></td>
                  <td>2026-05-22</td>
                  <td>编辑 · 预览 · 更多</td>
                </tr>
              </tbody>
            </table>
          </div>
        </section>
        <section class="qmai-panel">
          <div class="qmai-panel-head">
            <h4 class="qmai-panel-title">工作台 / 画布页模板</h4>
            <span class="qmai-tag qmai-tag-info">Canvas First</span>
          </div>
          <div class="qmai-layout-grid">
            <div class="qmai-canvas">
              <div class="qmai-receipt">
                <div class="qmai-row-title">票据预览</div>
                <div class="qmai-receipt-line" style="width:90%"></div>
                <div class="qmai-receipt-line" style="width:72%"></div>
                <div class="qmai-receipt-line" style="width:84%"></div>
                <div class="qmai-receipt-line" style="width:58%"></div>
              </div>
            </div>
            <div class="qmai-config">
              <div class="qmai-config-row">
                <div class="qmai-config-label">启用预览校验</div>
                <div class="qmai-switch"></div>
              </div>
              <div class="qmai-config-row">
                <div class="qmai-config-label">输出介质</div>
                <div class="qmai-field">80mm 热敏纸</div>
              </div>
              <div class="qmai-config-row">
                <div class="qmai-config-label">发布策略</div>
                <div class="qmai-field">保存后手动发布</div>
              </div>
            </div>
          </div>
          <div class="qmai-footer-bar">
            <span class="qmai-btn qmai-btn-danger">停用</span>
            <span class="qmai-btn">保存草稿</span>
            <span class="qmai-btn qmai-btn-primary">发布</span>
          </div>
        </section>
      </main>
    </div>
  </div>
</div>

## 设计 Token

现有仓库已有 theme CSS 变量体系，常见变量包括 `var(--color-primary)`、`var(--button-primary-font-color)`、`var(--button-primary-hover-background-color)`、`var(--theme)`、`var(--colorPrimary)`、`var(--bg-*)`、`var(--line-*)` 等。

新增或改造时优先复用当前仓库已有 token；没有可复用 token 时，局部新增变量必须可回退、语义清晰，并避免污染全局。

### 动作色

| 语义 | 推荐值 | 用途 |
| --- | --- | --- |
| Color Primary | `var(--color-primary)` | 默认主按钮、明确主 CTA、启用态、正向推进 |
| Primary Hover Background | `var(--button-primary-hover-background-color)` | hover / pressed / active 背景 |
| Primary Hover Border | `var(--button-primary-hover-border-color)` | hover / pressed / active 边框 |
| Primary Font | `var(--button-primary-font-color)` | 主按钮文字 |
| Primary Light | `var(--color-primary-light-9)` | 选中浅底、正向状态浅底 |

落地规则：

- 新页面主 CTA 优先使用项目现有 primary 组件，由仓库既有 theme CSS 变量文件接管按钮、radio、switch、input focus 等交互态颜色。
- 业务页面不要通过局部 `::v-deep` 覆盖 Element UI 交互组件主题色，除非是在修复明确的组件级问题。
- 如果当前仓库历史主色仍为 `#137dee` / Element 默认蓝色，不做无边界全局替换；优先跟随当前页面所在壳层实际注入的 `var(--color-primary)`。
- 不要在同一屏幕放多个同权重主按钮。

### 中性色

| 语义 | 推荐值 | 用途 |
| --- | --- | --- |
| Ink 900 | `#101828` | 页面标题、强标题 |
| Text 700 | `#344054` | 正文与次级交互文本 |
| Text 500 | `#667085` | 描述、元信息 |
| Text 400 | `#98A2B3` | placeholder、弱图标 |
| Border 300 | `#D0D5DD` | 标准边框 |
| Border 200 | `#EAECF0` | 分割线、弱边框 |
| Surface 50 | `#F8FAFB` | 表格 hover、弱分组 |
| Field Surface | `#F5F7F8` | 输入框静态底、工具底 |
| Canvas White | `#FFFFFF` | 主工作面 |

### 语义色

| 语义 | 推荐值 | 浅底 |
| --- | --- | --- |
| Info | `#2E90FA` | `#EFF8FF` |
| Warning | `#F79009` | `#FFFAEB` |
| Danger | `#D92D20` | `#FEF3F2` |

语义色只在意义变化时使用，不作为装饰色。危险操作必须明确结果，不能只靠红色表达风险。

## 排版与文案

新增局部样式不引入新的字体依赖。后台字体策略延续：

```scss
font-family: 'Helvetica Neue', Helvetica, 'PingFang SC', 'Hiragino Sans GB', 'Microsoft YaHei', '微软雅黑', Arial, sans-serif;
```

| 角色 | 尺寸 | 字重 | 用途 |
| --- | --- | --- | --- |
| 页面标题 | 24-32px | 700-800 | 页面一级任务标题 |
| 分区标题 | 18-22px | 700 | 大型配置区、编辑器分区 |
| 模块标题 | 16-18px | 600-700 | 卡片、设置组 |
| 正文 / 控件 | 14-16px | 400-600 | 主要阅读与操作 |
| 元信息 | 12-13px | 400-600 | 辅助说明、表头、标签 |

页面标题默认只允许：

- 只有标题
- `标题 + 一行有信息量的描述`

默认不允许：

- `小标签 + 标题 + 描述` 三层标题
- 页面自我介绍
- 没有操作价值的态度句
- 用说明文案弥补结构不清

## 页面骨架

### 列表页

结构：

- 页头：任务标题 + 主 CTA
- 查询区：筛选、搜索、重置、展开更多
- 表格主体：主信息、状态、关键字段、操作
- 分页 / 批量操作 / 弱反馈

规则：

- 表格是主角，不把列表页改成卡片墙。
- 查询按钮与新增按钮不能争夺同一主视觉位。
- 行内操作保持轻量，高风险操作进入确认链路。
- 空状态必须说明“当前没有什么”和“可以做什么”。
- 状态字段默认先做只读胶囊展示；启用、停用、复制、删除等变更动作可收进操作列更多菜单。

### 配置页

结构：

- 页面标题
- 可选 tab
- 左侧结构导航或顶部锚点
- 右侧配置主体
- 稳定保存路径

规则：

- 配置页必须像配置页，不做成 dashboard。
- 保存、取消、返回路径固定，不能随着滚动消失。
- 强约束配置要给规则说明，但说明必须短、准、可执行。

### 全屏编辑器

结构：

- 顶部条：返回、标题、状态、保存
- 中央单列正文或主编辑区
- 重复分区节奏
- 底部动作栏或固定保存区

规则：

- 深度编辑优先使用全屏编辑器，不把复杂表单塞进轻弹窗。
- 分区标题语法统一，不为每个分区发明装饰。
- 编辑器必须支持保存中、保存失败、离开确认等状态。
- 复杂业务选择器优先复用已有共享组件。

### 选择器

结构：

- 左侧结构树 / 分类 / 条件
- 右侧结果列表
- 明确选中态
- 已选汇总与确认

规则：

- 选中清晰度高于装饰性。
- 大量数据要考虑搜索、分页、虚拟列表或懒加载。
- 如果选择器来自 `vue-kylin`，优先修共享组件，不在业务侧复制一套。

### 工作台 / 画布页

适用于票据模板、打印模板、设计编辑器、预览驱动型配置器。

结构：

- 顶部轻控制条
- 中央真实预览 / 纸面 / 画布
- 旁侧配置器
- 保存、预览、发布、校验路径

规则：

- 画布是主角，配置器是辅助。
- 打印模板和票据预览必须优先贴近真实输出介质。
- 纸面预览默认白底、弱边框、弱阴影，不使用后台彩底块包装数据区。

### 数据总览页

结构：

- 顶部筛选条
- 核心 KPI
- 主趋势图
- 排行 / 异常关注
- 高频入口

规则：

- 数据页不是大屏，不使用过度装饰的图表秀场。
- 每个 KPI 必须有口径，口径复杂时提供 tooltip 或说明弹层。
- 异常数据优先帮助用户行动，不只展示颜色。

## 组件落地规则

### 按钮

- 主按钮使用项目 primary 组件，由既有 theme CSS 变量体系决定视觉。
- 同一区域只保留一个最高权重动作。
- 次按钮白底、标准边框、中性文本，不抢主按钮。
- 文本按钮只承担轻操作，不能组织整页主流程。
- 删除、停用、解绑等危险操作不使用主绿色，必须进入危险语义与确认链路。

### 输入与表单

- 输入框使用白底或 Field Surface，标准边框，focus 使用主动作色。
- 表单项错误、提示、禁用状态遵循组件库默认语义，不随意覆盖。
- 表单布局优先对齐 label 与输入区，长文本和多语言场景不固定死宽。
- 必填、校验、异步保存状态必须清晰。

### 表格

- 表头简洁，主字段左对齐稳定。
- 行 hover 轻量，不改变布局高度。
- 状态胶囊使用浅底 + 轻边框 + 可读文本。
- 操作列收敛为必要操作；超过 3 个操作时考虑更多菜单。
- 大表格优先保留主身份字段，响应式时隐藏次要列。

### 卡片与容器

- 普通卡片白底、轻边框，少阴影或无阴影。
- 不把每个信息块都包成大卡片。
- 不在卡片里再嵌套卡片。
- 12-16px 圆角用于标准卡片，20-24px 只给大容器、弹窗、工作台。

### 弹窗与抽屉

- 轻确认用弹窗，复杂编辑用全屏页或专门编辑器。
- 弹窗底部操作顺序与现有仓库保持一致。
- 高风险弹窗必须写清影响范围、不可逆性和确认条件。
- 抽屉用于旁路查看、轻编辑或选择流程，不承担整页复杂配置。

## qiankun 与跨仓归属

| 场景 | 优先检查 |
| --- | --- |
| 普通业务页面 | 对应 `*-vue-international` 仓库 |
| 菜单、面包屑、标签页、头部、侧栏、微应用挂载、跨应用跳转 | `console-vue-international` |
| 多仓库复现的表格、选择器、弹窗、上传、表单、搜索组件问题 | `vue-kylin-international` |
| `/commonCenter/`、共享设置页、公共业务能力页、打印模板、打印预览内容 | `common-vue-international` |

共享组件改动必须分析下游调用方，不做破坏性 API 变更。

## CSS 与工程约束

- 局部样式优先写在当前组件 scoped 样式里。
- 微前端全局样式必须加项目级前缀，避免污染其他子应用。
- 不为单个需求新增全局样式文件，除非已有模块模式要求。
- 不引入新的 UI 依赖或图标体系。
- 不大范围替换历史颜色 token。
- 不为了“统一风格”顺手格式化无关文件。
- 不使用彩色阴影、过大圆角、大面积浅彩背景制造高级感。
- 不让壳层背景、渐变、强阴影进入普通内容容器。

## i18n、RTL 与可访问性

### i18n

- 所有新增可见文案必须进入仓库既有 i18n 流程。
- 不用中文或英文硬编码替代翻译 key。
- 空状态、错误提示、按钮、tooltip、表格列名、确认弹窗都属于可见文案。

### RTL

- 避免只写 `margin-left`、`padding-left` 表达业务方向，优先使用 flex gap、逻辑属性或封装类。
- 图标箭头、步骤方向、左右双栏在 RTL 下需要额外检查。
- 表格主信息、金额、编号等字段需保持可读，不因 RTL 破坏扫描。

### 可访问性

- 颜色不能成为唯一状态表达，必须有文字或图标辅助。
- 主要按钮、危险操作、开关、校验信息必须可读。
- focus、loading、disabled、empty、error 状态必须完整。

## 响应式与桌面密度

国际后台默认桌面优先，但要避免只在单一 1440 宽度成立。

| 断点 | 范围 |
| --- | --- |
| 小屏 | `< 640` |
| 中屏 | `640-1023` |
| 桌面 | `1024-1279` |
| 大桌面 | `>= 1280` |

规则：

- 列表页优先隐藏次要列，不压缩主身份字段。
- 配置页左导航可转 drawer 或 tabs，但保存路径不能消失。
- 全屏编辑器保持单列，不压成并列迷你表单。
- 工作台先保留画布，再压缩辅助配置器。

## 评审清单

1. 这页属于哪个 archetype？
2. 主工作面是否明确？
3. 页面标题是否可以减少一层？
4. 文案是否都走 i18n？
5. 是否出现多个同权重主按钮？
6. 是否把列表页做成卡片墙？
7. 是否把壳层装饰带进内容区？
8. 是否用颜色替代了结构？
9. 保存、返回、确认路径是否稳定？
10. 是否需要联查 `console-vue-international`？
11. 是否需要联查 `vue-kylin-international`？
12. RTL 和长文本是否会撑破按钮、表格或筛选区？
13. 空、错、加载、禁用、保存中状态是否完整？

## AI 执行指令模板

处理国际后台 UI / UX / 样式需求时，可引用：

```text
请基于《Qmai International 后台设计技术规范》实现。先判断页面 archetype 与仓库归属，保持结构优先、白底工作面、绿色默认动作色、共享组件优先、i18n/RTL 兼容。不允许回退到通用 SaaS 卡片模板，不允许用解释性文案替代结构，不允许为单个页面发明独立视觉语言。
```

## 预览模板维护

源规范中还包含 Vue2 预览模板方向：

- `international-design-technical-spec/components/QmaiInternationalDesignSystemPreview.vue`
- `international-design-technical-spec/components/README.md`
- `international-design-technical-spec/standalone-preview-app/README.md`

用途：

- 作为按钮、表单、表格、状态、弹窗、工作台画布的统一视觉参考。
- 通过独立预览应用在浏览器中查看，或复制到目标业务仓库临时路由中做 UI 对照。
- 在 `vue-kylin-international` 组件样式调整前作为验收基线。

注意：模板是预览用，不是生产业务页面。复制到业务仓库后，所有可见文案必须替换为仓库 i18n key。
