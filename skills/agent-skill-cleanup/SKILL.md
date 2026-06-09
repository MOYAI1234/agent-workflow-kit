---
name: agent-skill-cleanup
description: "审计和清理多 Agent 工作区：断链 junction、临时文件、过期 skills、隔离归档。首次运行自动扫描建立基线。"
---

# Agent Skill Cleanup — 工作区清理

> 首次运行自动扫描工作区建立基线，后续运行对比基线找差异。

## 两种模式

| 模式 | 触发条件 | 做什么 |
|------|---------|--------|
| **初始化** | 无基线文件或用户说"建立基线"/"scan workspace" | 扫描工作区，记录当前状态作为基线 |
| **清理** | 基线已存在 | 对比基线找差异，执行清理 |

---

## 初始化流程（首次运行）

当基线文件不存在或用户要求重新建立时，执行以下扫描：

### 第一步：扫描 Skills 目录结构

扫描所有 skills 目录，记录每个目录的类型：

```bash
# 需要扫描的目录（根据 agent-paths.md 或自动检测）
~/.claude/skills/
~/.agents/skills/
~/.codex/skills/
~/.workbuddy/skills/
<workspace>/.workbuddy/skills/
```

对每个目录：
1. 检查是否存在 SKILL.md → 有则为真实 skill
2. 检查是否为 junction/symlink → 是则记录指向目标
3. 检查是否为空目录或只有 .gitkeep
4. 检查是否有 openai.yaml / _user_meta.json 等元数据

### 第二步：建立 Junction 关系图

```bash
# Windows: 检查 junction
cmd /c "dir /AL <skills-dir>"

# macOS/Linux: 检查 symlink
find <skills-dir> -type l -ls
```

记录每个 junction 的源路径和目标路径。

### 第三步：扫描工作区文件

扫描工作区根目录，记录：

```bash
# 零字节文件
find <workspace> -maxdepth 2 -empty -type f

# 临时文件模式
find <workspace> -maxdepth 2 \( -name "*.tmp" -o -name "*.bak" -o -name "*~" -o -name "*.swp" \)

# __pycache__ / .pytest_cache
find <workspace> -maxdepth 3 -type d \( -name "__pycache__" -o -name ".pytest_cache" \)

# 散落的 debug 脚本
find <workspace> -maxdepth 2 \( -name "debug_*" -o -name "test_*" -o -name "tmp_*" \) -type f
```

### 第四步：扫描文档引用

检查关键文档中引用的路径是否真实存在：

```bash
# 提取 CLAUDE.md / AGENTS.md 中引用的路径
grep -oE '[a-zA-Z]:\\[^\s"<>]+' <workspace>/CLAUDE.md
grep -oE '~\/[^\s"<>]+' <workspace>/CLAUDE.md
grep -oE '\.\/[^\s"<>]+' <workspace>/CLAUDE.md

# 逐一检查路径是否存在
```

### 第五步：生成基线文件

将扫描结果写入 `<workspace>/.workbuddy/memory/workspace-baseline.md`：

```markdown
# 工作区基线

> 自动生成于 YYYY-MM-DD。如需更新，对 agent 说"重新建立基线"。

## Skills 目录

### ~/.claude/skills/
- 真实 skill（15 个）：skill-a, skill-b, ...
- Junction（8 个）：skill-c → ~/.agents/skills/skill-c, ...
- 空目录（2 个）：skill-x, skill-y

### ~/.agents/skills/
- 真实 skill（20 个）：...
- Junction（5 个）：...

## 工作区文件

### 零字节文件（3 个）
- <workspace>/empty-file.txt
- ...

### 临时文件（5 个）
- <workspace>/debug_script.py
- ...

### 文档引用检查
- CLAUDE.md 引用 15 个路径，全部存在
- AGENTS.md 引用 12 个路径，2 个不存在（已标记）
```

### 第六步：输出摘要

向用户报告：
- 扫描了多少个 skills 目录
- 发现了多少个真实 skill / junction / 空目录
- 发现了多少个临时文件/零字节文件
- 文档引用检查结果
- 基线文件位置

---

## 清理流程（日常使用）

基线已存在时，按以下 7 步执行：

### 第一步：只读盘点

对比当前状态与基线：
- skills 数量是否变化
- junction 是否新增断链
- 临时文件是否新增

### 第二步：判断事实源

参照基线中的 skills 目录记录，确认哪些是真实源、哪些是镜像：
- 有 SKILL.md 且不是 junction → 真实源
- 是 junction 且目标存在 → 有效镜像
- 是 junction 但目标不存在 → 断链，可清理
- 空目录 → 可清理

### 第三步：低风险清理

- 删除断链的 junction
- 删除零字节文件
- 删除 `__pycache__`、`.pytest_cache` 等缓存目录
- 删除明显的临时文件（`*.tmp`、`*.bak`、`*~`、`*.swp`）

### 第四步：散落文件整理

- 调试脚本（`debug_*`、`test_*`、`tmp_*`）
- 路径错误的文件（放在不该放的位置）
- 一次性临时文档

### 第五步：隔离而不是硬删

大目录、历史备份、不确定的文件 → 移到 `_cleanup_quarantine/YYYYMMDD-<label>/`

### 第六步：复查和记录

- 确认断链清零
- 更新基线文件
- 写入清理日志

### 第七步：工具链同步

参照 `SYNC.md` 检查是否需要更新 `docs/tools-inventory.md` 和 `tool-router` 的 `REFERENCE.md`。

---

## 红线（绝对不碰）

- 真实源 skills（只清理镜像，不清理源）
- 运行态数据（数据库、OAuth state、config）
- `.venv`、`node_modules`（能重装但费时间）
- 任何 `.git` 目录
- **所有删除/移动操作必须用户确认**
- `.venv` 和临时文件不同批次清理
