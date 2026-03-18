---
name: scp-frontend-design
description: SCP 供应链计划系统前端设计规范。白底+天蓝风格，提供色彩、字体、组件、布局等设计令牌与组件规范。当用户要求设计/开发 SCP 前端页面、组件、原型或遵循系统设计风格时引用本 skill。
---

# SCP 前端设计规范

供应链计划系统（SCP）的通用前端设计规范，用于保持页面风格一致。新建或改版页面时应遵循本规范。

## 一、视觉方向

- **风格**：白色 + 天蓝色，简洁专业
- **主题色**：`#0ea5e9`（天蓝）作为主色，`#22c55e`（成功绿）、`#ef4444`（失败红）、`#f59e0b`（警示黄）作为状态色

## 二、CSS 变量（设计令牌）

```css
:root {
  --page-bg: #ffffff;
  --card-bg: #ffffff;
  --card-border: #e2e8f0;
  --header-bg: #f8fafc;
  --primary: #0ea5e9;
  --primary-hover: #38bdf8;
  --primary-light: rgba(14, 165, 233, 0.08);
  --accent: #0ea5e9;
  --success: #22c55e;
  --success-bg: rgba(34, 197, 94, 0.1);
  --error: #ef4444;
  --error-bg: rgba(239, 68, 68, 0.1);
  --warn: #f59e0b;
  --text: #1e293b;
  --text-muted: #64748b;
  --radius: 10px;
  --radius-sm: 6px;
  --shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
  --shadow-hover: 0 4px 12px rgba(14, 165, 233, 0.15);
  --transition: 0.2s cubic-bezier(0.4, 0, 0.2, 1);
}
```

## 三、字体

| 用途 | 字体 | 说明 |
|------|------|------|
| 标题与正文 | Outfit, Noto Sans SC | 英文 + 中文 |
| 数字与代码 | JetBrains Mono | 统计值、ID、代码块 |

```html
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500;600&family=Outfit:wght@400;500;600;700&family=Noto+Sans+SC:wght@400;500;600;700&display=swap" rel="stylesheet" />
```

```css
body { font-family: 'Outfit', 'Noto Sans SC', sans-serif; font-size: 14px; line-height: 1.5; }
.stat-value, .detail-value, .code-block { font-family: 'JetBrains Mono', monospace; }
```

## 四、背景与边框

- **主背景**：`#ffffff`
- **表头/分页区**：`#f8fafc`
- **边框**：`#e2e8f0`
- **行分隔线**：`#f1f5f9`

## 五、圆角与阴影

- **卡片/按钮**：`--radius: 10px`
- **小圆角**：`--radius-sm: 6px`
- **默认阴影**：`0 1px 3px rgba(0, 0, 0, 0.08)`
- **悬停阴影**：`0 4px 12px rgba(14, 165, 233, 0.15)`

## 六、状态色与状态点

| 状态 | 颜色 | 用途 |
|------|------|------|
| 成功 | `#22c55e` | 成功绿 |
| 失败 | `#ef4444` | 失败红 |
| 警示 | `#f59e0b` | 部分成功、待处理 |
| 待处理 | `#64748b` | 灰色 |

状态点：6px 圆点 + 文字，`.status-dot.success::before { background: var(--success); }`

## 七、组件规范

### 7.1 页面结构

- **顶部**：页面标题 + Tab（可选）
- **主内容区**：padding 24px 32px
- **单页 Tab**：顶部标题 + Tab 切换，合并为一个菜单入口

### 7.2 页面标题

- **标题**：1.5rem、700、letter-spacing -0.02em
- **描述**：13px、`--text-muted`

### 7.3 筛选栏（toolbar）

- 背景：`--card-bg`，边框：`--card-border`，圆角：`--radius`
- padding：16px 20px，gap：16px
- 支持 `header-with-toolbar`：标题与筛选栏同一行，筛选栏右对齐

### 7.4 表单控件

- **label**：`font-weight: 500`，`color: var(--text-muted)`，13px
- **select/input**：padding 8px 14px，border-radius `--radius-sm`，min-width 140px
- **focus**：`border-color: var(--primary)`

### 7.5 按钮

| 类型 | 样式 |
|------|------|
| 主按钮 | `--primary` 背景，hover 时 `--primary-hover` + transform translateY(-1px) |
| 次要按钮 | 透明背景、边框，hover 时 `--primary` 文字与边框 |
| 链接按钮 | 边框 + 浅蓝背景，hover 时加深 |

### 7.6 统计卡片（stat-card）

- 4 列网格，gap 16px
- 卡片：`border: 1px solid var(--card-border)`，hover 时 `border-color: var(--primary)`、`box-shadow: var(--shadow-hover)`
- 标签：12px、uppercase、letter-spacing 0.04em
- 数值：JetBrains Mono、1.5rem、600
- 状态色：`.stat-value.success`、`.stat-value.error`、`.stat-value.warn`、`.stat-value.primary`

### 7.7 表格

- **表头**：`--header-bg` 背景，12px、uppercase、letter-spacing 0.03em
- **单元格**：padding 12px 16px
- **行悬停**：`background: var(--primary-light)`
- **可点击行**：`cursor: pointer`
- **选中行**：`background: var(--primary-light) !important`

### 7.8 抽屉（drawer）

- 右侧滑出，560px 宽（可扩展至 680px）
- 遮罩：`rgba(0, 0, 0, 0.35)`
- 阴影：`-8px 0 32px rgba(0, 0, 0, 0.12)`
- 详情行：label 120px 固定宽，value 等宽字体

### 7.9 分页

- 背景：`--header-bg`，边框：`--card-border`
- padding：12px 20px
- 激活页码：`--primary` 背景

### 7.10 错误/状态展示

- **错误块**：`--error-bg` 背景，`rgba(239, 68, 68, 0.25)` 边框
- **成功块**：`--success-bg` 背景
- **代码块**：`--header-bg` 背景，JetBrains Mono，11px

## 八、布局模式（按需选用）

以下为可选布局，根据页面业务选择，非强制。

### 8.1 标题 + 筛选栏同一行

```html
<div class="header-with-toolbar">
  <div class="page-header">
    <h1 class="page-title">页面标题</h1>
    <p class="page-desc">页面描述</p>
  </div>
  <div class="toolbar">...</div>
</div>
```

### 8.2 汇总 + 联动明细（按需使用）

当业务需要主从联动时可采用：汇总表在上、明细表在下；点击汇总行时明细表联动更新；选中行使用 `tr.selected` 样式。非必需，按场景选择。

## 九、适用场景（按需选用）

本规范可覆盖但不限于：列表+筛选+详情下钻、统计卡片+表格+抽屉、多 Tab 单页视图、表单/弹窗/状态展示等。具体采用哪些组件与布局，根据业务需求选择。
