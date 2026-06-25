# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 仓库定位

这是一个**个人学习仓库**。用户用 AI 驱动的 1v1 私教式学习替代传统博客笔记：通过不断向 Claude 提问，将理解后的知识输出为结构化的 HTML 文件。仓库不限技术主题，任何知识都可以。

## 核心学习流程：先提纲，后展开

**这是最重要的规则。** 用户的学习方式分两步：

### 第一步：制作提纲

对于一个新主题，首先产出**提纲页面**——仅包含大纲结构和核心概念罗列，不展开细节。典型格式见 `frontend/nodejs-guide.html`：

```html
<!-- 页面开头用 .section-nav 列出提纲 -->
<div class="section-nav">
  <strong>📑 目录</strong>
  <ul>
    <li><a href="#part1">第一部分：xxx</a> — 一句话描述</li>
    <li><a href="#part2">第二部分：xxx</a> — 一句话描述</li>
  </ul>
</div>

<!-- 提纲内容用 <h2> 章节标题 + 要点列表/表格，不展开深层细节 -->
<h2>一、语言基础（门槛）</h2>
<table>
  <tr><th>主题</th><th>精通标准</th></tr>
  <tr><td>异步模型</td><td>Event Loop 六阶段时序了然于心；能解释 nextTick vs setImmediate 的执行顺序</td></tr>
</table>
```

### 第二步：基于提纲提问，展开具体条目

用户会针对提纲中的某个条目提问。**只有提纲中已有的条目才需要展开**到 HTML 中。展开后的内容追加为新的 `<h2>` section，放在对应提纲条目附近。例如：

```
提纲有: <h2>一、语言基础</h2> → 含"异步模型"条目
用户问: "Event Loop 六阶段详细讲一下"
→ 在"一、语言基础"之后插入 <h2>一·附：Event Loop 六阶段详解（源码级）</h2>
```

**规则：**
- 用户随口问的、不在提纲里的话题 → 正常回答，但**不追加到 HTML**
- 用户针对提纲条目的深入提问 → 回答后**追加到对应 HTML 的相应位置**
- 追加的新 section 格式：`<h2>X·附：具体展开标题</h2>`（X 为所属章节编号）
- 新增或拆分章节/知识点时必须**同步更新目录**（`.section-nav`），保证目录与内容一致
- 如果用户在项目里问了某个话题，且恰好有对应的 HTML 文件（或文件内的章节），则回答后须**同步更新**到该 HTML 中

## 文件组织

```
learning2-sapphire611/
├── README.md
├── frontend/
│   └── nodejs-guide.html      # Node.js 学习指南
├── backend/
│   ├── vue-guide.html          # Vue 3 学习指南
│   └── react-guide.html        # React 学习指南
└── （按需增加子目录和 html）
```

- 文件命名：`<topic>-guide.html`（英文小写、连字符分隔）
- 按领域分子目录（`frontend/`、`backend/`、`devops/`、`cs-basics/` 等）
- 每个 HTML 文件自包含（内联 CSS、无外部依赖）

## HTML 模板规范

所有 HTML 文件共享同一套视觉设计体系。新建文件时必须复用。

### CSS 变量（:root）

```css
--bg: #0d1117;       /* 页面背景 */
--card: #161b22;     /* 卡片背景 */
--border: #30363d;   /* 边框 */
--text: #c9d1d9;     /* 正文 */
--muted: #8b949e;    /* 次要文字 */
--accent: #58a6ff;   /* h3 标题色 */
--green: #3fb950;    /* .tip 边框 */
--yellow: #d2991d;   /* .note 边框 */
--red: #f85149;      /* .warn 边框 */
--purple: #bc8cff;   /* h4 标题色 */
--orange: #f0883e;   /* inline code 颜色 */
--cyan: #39d2c0;
```

每个主题文件可额外定义主题色变量（如 `--vue: #42b883`），用于 h2 标题的左边框和文字色。

### 语法高亮 class（用在 `<span>` 上）

- `.cm` — 注释（`#8b949e`, italic）
- `.kw` — 关键字（`#ff7b72`）
- `.str` — 字符串（`#a5d6ff`）
- `.fn` — 函数名（`#d2a8ff`）
- `.num` — 数字（`#79c0ff`）

### 内容组件

| Class | 用途 |
|-------|------|
| `.section-nav` | 页面开头的提纲目录（链接到各部分） |
| `.card` | 强调/总结框（深色背景 + 边框） |
| `.note` | 备注/注意（黄色左边框） |
| `.warn` | 警告/陷阱（红色左边框） |
| `.tip` | 技巧/最佳实践（绿色左边框） |
| `.footer` | 页脚（居中、灰色、上边框） |

### 页面布局

- `<body>` max-width: 1100px，居中
- `<h1>` 白色，底部分割线；多部分页面可在 body 内使用多个 `<h1>` 标题
- `<h2>` 主题色/accent，左侧 4px 边框，作为每个大章节的标题
- `<h3>` accent/green 色，子章节标题
- `<table>` 全宽、斑马纹、hover 高亮
- `<pre><code>` 深色背景、等宽字体、带边框圆角

### footer 格式

```html
<div class="footer">
  Generated with Claude Code · YYYY-MM-DD
</div>
```
新追加内容时，在最后一个 `</body>` 之前插入，保持 footer 在最末尾。
