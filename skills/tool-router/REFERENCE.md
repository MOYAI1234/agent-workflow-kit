# Tool Router — 决策树详情

> 本文件是 SKILL.md 的参考文档。Agent 在需要定位工具时读取本文件。
>
> **使用方式**：复制本文件，根据你实际使用的工具填充决策树分支。标注 `<!-- TODO -->` 的地方需要你填写。

## 决策树

### 1. 数据/报告

```
要写报告？
├── 日报 → /daily-report（skill）
├── 周报 → /weekly-report（skill）
├── 月报 → /monthly-report（skill）
├── 版本报告 → /version-report（skill）
├── 运营晨报 → /moyai-ops（skill）
└── 查原始数据？
    ├── 数据平台 → /ta-analytics（skill）或 API 直调
    └── 行情/财务 → /mx-data（skill）<!-- TODO: 替换为你实际的数据工具 -->
```

### 2. 知识库

```
要查/写知识库？
├── <!-- TODO: 你的知识库 1 --> → SSH kb-1 CLI
├── <!-- TODO: 你的知识库 2 --> → SSH kb-2 CLI
├── 通用知识搜索 → IMA 知识库（MCP connector）
├── 碎片笔记/日记 → Obsidian（/mp-obsidian-vault）
└── 不确定用哪个？
    ├── <!-- TODO: 领域 1 --> → kb-1
    ├── <!-- TODO: 领域 2 --> → kb-2
    ├── 快速搜索 → IMA
    └── 个人记录 → Obsidian
```

### 3. 飞书操作（如适用）

```
要操作飞书？
├── 发消息/群聊 → lark-im
├── 文档读写 → lark-doc
├── 电子表格 → lark-sheets
├── 多维表格 → lark-base
├── 任务管理 → lark-task
├── 日历/会议 → lark-calendar
├── 云盘文件 → lark-drive
├── 知识库/Wiki → lark-wiki
├── 审批 → lark-approval
├── OKR → lark-okr
├── 邮件 → lark-mail
├── 会议纪要 → lark-minutes
├── 幻灯片 → lark-slides
├── 画板 → lark-whiteboard
├── 考勤打卡 → lark-attendance
├── 查人/通讯录 → lark-contact
├── 视频会议 → lark-vc
├── 部署 HTML → lark-apps
├── 原生 API 探索 → lark-openapi-explorer
├── 造新飞书 Skill → lark-skill-maker
└── 所有操作前必须先读 → lark-shared（认证/权限）
```

### 4. 浏览器/网页

```
要操作网页？
├── 需要登录态 → chrome-mcp-server
├── 反爬/Cloudflare 保护 → CloakBrowser（Python）
├── 轻量抓取 → lightpanda MCP
└── 标准自动化 → Playwright
```

### 5. 社交平台/网页信息获取

```
要读社交平台或网页信息？
├── 搜推特/微博/小红书/B站/Reddit → Agent-Reach（/agent-reach）
├── 读网页/文章/公众号 → Agent-Reach（Jina Reader / Exa）
├── YouTube/B站字幕提取 → Agent-Reach（yt-dlp）
├── GitHub 代码搜索/仓库信息 → Agent-Reach（gh CLI）或 github connector
├── 需要登录态操作后台 → chrome-mcp-server
├── 反爬网站 → CloakBrowser
└── 不确定？→ 先试 Agent-Reach（覆盖面最广）
```

### 6. 代码工程

```
要做代码工程？
├── 代码审查 → /mp-review
├── 排障诊断 → /mp-diagnose
├── 写测试 → /mp-tdd
├── 做原型 → /mp-prototype
├── 拆 Issue → /mp-to-issues
├── 写 PRD → /mp-to-prd
├── Issue 分诊 → /mp-triage
├── 架构改进 → /mp-improve-architecture
├── 连续追问对齐 → /mp-grill-me
├── 安全 Git 钩子 → /mp-git-guardrails-claude-code
├── 会话交接 → /mp-handoff
├── 极简模式（省 token）→ /mp-caveman
└── 不熟悉代码 → /mp-zoom-out（先看全局地图）
```

### 7. 内容创作

```
要写/改内容？
├── 去 AI 味 → /humanizer
├── 编辑文章 → /mp-edit-article
├── 章节塑形 → /mp-writing-shape
├── 碎片挖掘 → /mp-writing-fragments
├── 节拍写作 → /mp-writing-beats
├── 前端界面 → /frontend-design
├── UI 动画 → /ui-animation
└── 问卷调查 → /typeform-survey-builder
```

### 8. 文档/图片 OCR

```
要把图片/PDF 转成文字？
├── 多语言 OCR → PaddleOCR
├── 中文 PDF 深度解析（表格/公式/版面）→ deepdoc-toolkit
└── 不确定 → 先试 PaddleOCR（覆盖更广）
```

### 9. 金融/投资（如适用）

```
要查金融数据？
├── <!-- TODO: 你的金融数据工具 -->
├── <!-- TODO: 你的模拟交易工具 -->
└── <!-- TODO: 你的投资知识库 -->
```

### 10. 项目管理

```
要管项目？
├── 飞书任务 → /feishu-pm 或 lark-task
├── Git PR → gh CLI + github connector
├── 文档同步 → /neat-freak
└── 清理 skills → /agent-skill-cleanup
```

### 11. <!-- TODO: 你的自定义分类 -->

```
<!-- TODO: 根据你的实际工具添加新分类 -->
```

## Agent 归属速查

> 根据你使用的 Agent 平台填写此表。

| 工具类型 | Agent A | Agent B | Agent C |
|---------|---------|---------|---------|
| <!-- TODO: 填写你的工具归属 --> | | | |

## 维护指南

### 增加新工具

1. 安装 skill/plugin/MCP
2. 编辑 `docs/tools-inventory.md` 添加条目
3. 编辑本文件在对应决策树节点添加分支

### 删除工具

1. 确认无其他工具依赖
2. 从 `tools-inventory.md` 移除
3. 从本文件移除
4. 清理 skill 目录（参考 `/agent-skill-cleanup`）

### 检查冲突

同类工具超过 2 个时，必须在 `tools-inventory.md` 的对比表中写清楚差异和选择规则。
