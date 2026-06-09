---
name: neat-freak
description: "项目收尾时同步文档和记忆。做完事 → /neat → 切换/结束。首次运行自动检测 Agent 平台和路径。"
---

> Based on [neat-freak](https://github.com/KKKKhazix/Khazix-Skills/tree/main/neat-freak) by 数字生命卡兹克, MIT License.

# Neat Freak — 收尾文档同步

> 做完事就跑 `/neat`，像写完代码要 git commit 一样自然。
>
> 自动生成的文件：
> - `references/agent-paths.md` — Agent 平台路径速查（首次运行自动检测）

## 两种模式

| 模式 | 触发条件 | 做什么 |
|------|---------|--------|
| **初始化** | `references/agent-paths.md` 不存在或用户说"检测路径" | 自动扫描系统，检测 Agent 平台和配置文件位置 |
| **同步** | `references/agent-paths.md` 已存在 | 正常的文档同步流程 |

---

## 初始化流程（首次运行）

当 `references/agent-paths.md` 不存在或用户要求重新检测时，执行以下扫描：

### 第一步：检测 Agent 平台

逐一检查以下平台是否存在：

```bash
# Claude Code
ls ~/.claude/CLAUDE.md 2>/dev/null
ls ~/.claude/skills/ 2>/dev/null

# OpenAI Codex
ls ~/.codex/AGENTS.md 2>/dev/null
ls ~/.codex/skills/ 2>/dev/null
ls ~/.agents/skills/ 2>/dev/null

# WorkBuddy
ls ~/.workbuddy/MEMORY.md 2>/dev/null
ls ~/.workbuddy/skills/ 2>/dev/null

# Trae / SOLO
ls ~/.trae/ 2>/dev/null

# OpenClaw
ls ~/.openclaw/CLAUDE.md 2>/dev/null
ls ~/.openclaw/skills/ 2>/dev/null
```

### 第二步：检测工作区配置文件

对每个检测到的平台，找到其配置文件位置：

```bash
# 全局配置
~/.claude/CLAUDE.md
~/.codex/AGENTS.md
~/.workbuddy/MEMORY.md
~/.workbuddy/SOUL.md
~/.workbuddy/IDENTITY.md

# 项目级配置（在工作区根目录）
<workspace>/CLAUDE.md
<workspace>/AGENTS.md
<workspace>/.claude/settings.json
<workspace>/.workbuddy/memory/
```

### 第三步：检测记忆文件位置

```bash
# Claude Code 项目记忆
~/.claude/projects/<project-hash>/memory/

# WorkBuddy 记忆
<workspace>/.workbuddy/memory/
<workspace>/.workbuddy/memory/YYYY-MM-DD.md
<workspace>/.workbuddy/memory/MEMORY.md

# 通用 docs 目录
<workspace>/docs/memory/
```

### 第四步：生成 agent-paths.md

将检测结果写入 `references/agent-paths.md`，格式：

```markdown
# Agent 平台路径速查

> 自动生成于 YYYY-MM-DD。如需更新，对 agent 说"重新检测路径"。

## 已检测到的平台

### Claude Code
- 全局配置：~/.claude/CLAUDE.md
- Skills 目录：~/.claude/skills/
- 项目配置：<workspace>/CLAUDE.md
- 记忆文件：~/.claude/projects/<hash>/memory/

### WorkBuddy
- 全局记忆：~/.workbuddy/MEMORY.md
- Skills 目录：~/.workbuddy/skills/
- 项目记忆：<workspace>/.workbuddy/memory/

（... 每个检测到的平台都列出 ...）

## 未检测到的平台

- OpenAI Codex：未安装
- Trae：未安装
（... 列出未检测到的平台 ...）
```

### 第五步：输出摘要

向用户报告：
- 检测到了哪些平台
- 每个平台的配置文件位置
- 提示用户检查是否准确

---

## 同步流程（日常使用）

`references/agent-paths.md` 已存在时，按以下流程执行：

### 触发方式

| 模式 | 触发 | 适用场景 |
|------|------|---------|
| **dry-run** | `/neat --dry-run` | 只审查，不修改文件 |
| **light** | `/neat` | 日常小步开发收尾（默认） |
| **full** | `/neat --full` | 大功能完成、交接、全量整理 |

### 执行步骤

1. **Git 安全检查**
   - `git status --short` — 查看哪些文件被修改
   - `git diff --stat` — 查看修改规模
   - 如有未提交的改动，先 commit（但不 push）

2. **盘点现状**
   - light：查根目录 + 相关 docs
   - full：查所有 .md 文件、docs/ 目录、记忆文件

3. **识别变更**
   - 读 `references/sync-matrix.md`（变更影响矩阵）
   - 对照 git diff 判断哪些文档需要更新

4. **实际修改**
   - **必须真的用工具修改文件**
   - 修改顺序：docs/ → 配置文件 → 记忆文件

5. **输出变更摘要**
   - 模式、Git 状态、文档变更、验证、未处理项

### 编辑原则

- **合并优于追加** — 文档是给人读的，不是日志
- **删除优于保留** — 过期信息比没有信息更糟糕
- **精确优于冗长** — 每句话都要有信息量
- **绝对时间** — 永远写 `2026-06-09`，不写"今天"
- **面向读者** — 想象对方只有 5 分钟能看完
