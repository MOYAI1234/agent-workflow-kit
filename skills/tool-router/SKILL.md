---
name: tool-router
description: "工具链决策树 — 做 X 该用什么工具？首次运行自动扫描工作区构建决策树，后续运行查询/维护工具清单。"
---

# Tool Router — 工具链决策树

> 自动生成的文件：
> - `docs/tools-inventory.md` — 完整工具清单
> - `REFERENCE.md` — 决策树详情

## 两种模式

| 模式 | 触发条件 | 做什么 |
|------|---------|--------|
| **初始化** | REFERENCE.md 不存在或用户说"扫描"/"初始化"/"rebuild" | 自动扫描工作区，生成工具清单和决策树 |
| **查询/维护** | REFERENCE.md 已存在 | 读现有决策树帮用户定位工具 |

---

## 初始化流程（首次运行）

当 REFERENCE.md 不存在或用户要求重建时，执行以下扫描：

### 第一步：扫描 Skills 目录

扫描所有 agent 平台的 skills 目录，读取每个 skill 的 SKILL.md 获取名称和用途：

```bash
# Claude Code
~/.claude/skills/*/
# Codex / .agents
~/.agents/skills/*/
# Codex 自身
~/.codex/skills/*/
# WorkBuddy user-level
~/.workbuddy/skills/*/
# WorkBuddy project-level
<workspace>/.workbuddy/skills/*/
```

对每个目录：
1. 检查是否存在 SKILL.md
2. 读取 frontmatter 中的 `name` 和 `description`
3. 记录路径和来源平台

### 第二步：扫描 MCP 连接器

检查各平台的 MCP 配置：

```bash
# Claude Code MCP
~/.claude/mcp.json 或 ~/.claude/settings.json
# WorkBuddy MCP
~/.workbuddy/mcp.json
# Codex MCP
~/.codex/mcp.json 或类似配置
```

解析 JSON，提取每个 MCP server 的名称、类型、用途。

### 第三步：扫描 CLI 工具

检查 PATH 中可用的 CLI 工具：

```bash
# 常见 agent 相关 CLI
which gh git lark-cli node python pip docker kubectl
# 检查各工具 --version 或 help
```

只记录与工作流相关的工具，忽略系统基础工具。

### 第四步：扫描工作区项目

扫描用户指定的工作区根目录，识别活跃项目：

```bash
# 查找项目标识文件
<workspace>/*/package.json    → Node.js 项目
<workspace>/*/pyproject.toml  → Python 项目
<workspace>/*/Cargo.toml      → Rust 项目
<workspace>/*/go.mod          → Go 项目
<workspace>/*/Makefile        → Make 项目
<workspace>/*/.git            → Git 仓库
```

### 第五步：生成工具清单

将扫描结果写入 `docs/tools-inventory.md`，按以下分类组织：

1. **Skills** — 按功能分类（业务/知识库/飞书/金融/开发/内容/其他）
2. **MCP 连接器** — 按状态分类（已连接/已断开）
3. **CLI 工具** — 按用途分类
4. **项目目录** — 按领域分类
5. **平台归属** — 哪个 skill 在哪个 agent 平台可用

### 第六步：生成决策树

基于工具清单，自动生成 REFERENCE.md 决策树：

1. 按功能域分类（数据/知识库/浏览器/代码/内容/...）
2. 每个分类下，按使用场景细分
3. 同类工具超过 2 个时，生成对比表
4. 生成 Agent 归属速查表

**决策树格式**：

```
要做什么？
├── 场景A → 工具1（skill）
├── 场景B → 工具2（CLI）
├── 场景C
│   ├── 子场景1 → 工具3
│   └── 子场景2 → 工具4
└── 不确定？→ 先试工具5（覆盖面最广）
```

### 第七步：输出摘要

向用户报告：
- 扫描了多少个目录
- 发现了多少个 skills / MCP / CLI / 项目
- 按分类列出概览
- 提示用户检查生成的文件是否准确

---

## 查询流程（日常使用）

REFERENCE.md 已存在时：

### 用户不知道用什么工具

```
1. 读 REFERENCE.md，按用户意图匹配分类
2. 在决策树中定位具体工具
3. 如有多个候选，按对比表推荐最优
4. 如需详情，读 tools-inventory.md 对应条目
5. 执行用户任务
```

### 用户要增删改工具

```
1. 增加：安装 → 编辑 tools-inventory.md → 编辑 REFERENCE.md 决策树
2. 删除：确认无依赖 → 从 tools-inventory.md 移除 → 从 REFERENCE.md 移除 → 清理目录
3. 修改：更新 tools-inventory.md → 更新 REFERENCE.md
4. 同类工具超 2 个时，必须在 tools-inventory.md 写对比表
```

### 用户要查工具冲突

```
1. 读 tools-inventory.md 的「问题和待处理」章节
2. 读 REFERENCE.md 的「Agent 归属速查」表
3. 指出重叠和独占关系
```

---

## 维护规则

- 增加新工具：安装 → 编辑 `tools-inventory.md` → 编辑 `REFERENCE.md`
- 删除工具：确认无依赖 → 从两个文件移除 → 清理目录
- 同类工具超 2 个时，必须在 `tools-inventory.md` 写对比表
- 工具清单是唯一事实源，三个 skill（tool-router / neat-freak / agent-skill-cleanup）都从这里读
