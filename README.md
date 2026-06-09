# Agent Workflow Kit

一套 AI 编码 Agent 工作流管理 skills 集合。装上之后，agent 会自动扫描你的工作区，构建一套完整的工具管理和文档维护流程。

解决三个问题：

1. **做 X 该用什么工具？** — `tool-router` 工具链决策树
2. **做完事怎么同步？** — `neat-freak` 收尾文档同步
3. **定期怎么清理？** — `agent-skill-cleanup` 工作区深度清洁

## 谁需要这个

- 用 Claude Code / Codex / WorkBuddy 等 AI 编码 Agent 的人
- 工作区里有多个项目、多个 skills、多个 agent 平台的人
- 受够了"这个工具在哪"、"文档没同步"、"断链满天飞"的人

## 快速开始

### 1. 安装 skills

```bash
# Claude Code
cp -r skills/tool-router ~/.claude/skills/
cp -r skills/neat-freak ~/.claude/skills/
cp -r skills/agent-skill-cleanup ~/.claude/skills/

# Codex / .agents
cp -r skills/tool-router ~/.agents/skills/
cp -r skills/neat-freak ~/.agents/skills/
cp -r skills/agent-skill-cleanup ~/.agents/skills/
```

### 2. 首次运行（自动发现）

对你的 agent 说：

```
扫描工作区，初始化工具链
```

agent 会自动完成：

| 步骤 | 做什么 | 产出 |
|------|--------|------|
| 1 | 扫描 skills 目录（读每个 SKILL.md） | 工具清单 |
| 2 | 扫描 MCP 连接器配置 | 工具清单 |
| 3 | 扫描 PATH 中的 CLI 工具 | 工具清单 |
| 4 | 扫描工作区项目目录 | 工具清单 |
| 5 | 检测 Agent 平台 | 路径速查 |
| 6 | 生成决策树 + 补充选择建议 | 决策树 |
| 7 | 建立工作区基线 | 基线文件 |

**整个过程自动完成，不需要手动填写任何模板。**

### 3. 开始用

```
"我想做数据报表，该用什么工具？"  → tool-router 帮你定位
/neat                              → neat-freak 同步文档和记忆
"清理一下工作区"                   → agent-skill-cleanup 安全清理
```

## 生成的文件

首次扫描完成后，你的工作区会多出这些自动生成的文件：

```
<workspace>/
├── docs/
│   ├── tools-inventory.md          # 完整工具清单（skills/MCP/CLI/项目）
│   └── memory/
│       └── workspace-baseline.md   # 工作区基线（用于清理对比）
├── skills/tool-router/
│   └── REFERENCE.md                # 决策树（含选择建议）
└── skills/neat-freak/references/
    └── agent-paths.md              # Agent 平台路径速查
```

之后装了新工具，说"更新工具链"就行，增量扫描，不会覆盖你手动改过的内容。

## 目录结构

```
agent-workflow-kit/
├── README.md
├── LICENSE                         # MIT
├── CHANGELOG.md
├── skills/
│   ├── tool-router/                # 工具链决策树
│   │   ├── SKILL.md                # 入口（含初始化流程）
│   │   └── REFERENCE.md            # 决策树（首次运行自动生成）
│   ├── neat-freak/                 # 收尾文档同步
│   │   ├── SKILL.md                # 入口（含初始化流程）
│   │   └── references/
│   │       ├── agent-paths.md      # Agent 路径（首次运行自动生成）
│   │       └── sync-matrix.md      # 变更影响矩阵（通用）
│   └── agent-skill-cleanup/        # 工作区清理
│       ├── SKILL.md                # 入口（含初始化流程）
│       ├── REFERENCE.md            # 详细规则
│       └── SYNC.md                 # 工具链同步指南
└── examples/
    └── my-workflow.md              # 使用示例
```

## 三个 Skill 的关系

```
首次安装                     日常使用
──────────                   ────────
"扫描工作区"                 "我想做 X"
      │                           │
      ▼                           ▼
 自动发现                    tool-router
 skills/MCP/CLI              读决策树定位工具
      │                           │
      ▼                           ▼
 生成清单 + 决策树            做完事 → /neat
 生成基线                     neat-freak 同步文档
      │                           │
      └──── 共享：tools-inventory.md ────┘
                 （唯一事实源）
                        │
                        ▼
                  定期清理
                  agent-skill-cleanup
                  对比基线找差异
```

## 自动发现机制

### tool-router 发现什么

| 扫描目标 | 扫描方式 | 产出 |
|---------|---------|------|
| Skills 目录 | 遍历 `~/.claude/skills/`、`~/.agents/skills/` 等，读 SKILL.md | 每个 skill 的名称、用途、来源平台 |
| MCP 连接器 | 读 `~/.workbuddy/mcp.json` 等配置文件 | 每个 MCP 的名称、类型、状态 |
| CLI 工具 | `which` + `--version` | PATH 中可用的 agent 相关工具 |
| 项目目录 | 扫描工作区根目录，识别 package.json/pyproject.toml 等 | 活跃项目列表 |

生成决策树后，还会根据每个 skill 的 SKILL.md description 自动补充选择建议（"不确定用哪个？先试 X"）。

### neat-freak 发现什么

| 扫描目标 | 扫描方式 | 产出 |
|---------|---------|------|
| Agent 平台 | 检查 `~/.claude/`、`~/.codex/`、`~/.workbuddy/` 等 | 已安装的平台列表 |
| 配置文件 | 检查每个平台的 CLAUDE.md / AGENTS.md / MEMORY.md 等 | 配置文件路径速查 |
| 记忆文件 | 检查 `docs/memory/`、`.workbuddy/memory/` 等 | 记忆文件位置 |

### agent-skill-cleanup 发现什么

| 扫描目标 | 扫描方式 | 产出 |
|---------|---------|------|
| Skills 结构 | 遍历目录，区分真实源/junction/空目录 | skills 关系图 |
| Junction 关系 | 检测 symlink/junction 指向 | 断链检测基线 |
| 临时文件 | 按模式匹配（`*.tmp`、`debug_*` 等） | 可清理文件清单 |
| 文档引用 | 从 CLAUDE.md 等提取路径并验证 | 引用完整性检查 |

## 设计原则

- **自动发现**：首次运行自动扫描，不需要手动填写模板
- **唯一事实源**：工具清单是三个 skill 共享的核心
- **安全优先**：所有删除/移动操作必须用户确认，隔离优先于硬删
- **平台无关**：不绑定特定 Agent 平台，通过自动检测适配
- **渐进式**：三个 skill 各自独立，可以只装一个，也可以全装

## 致谢

- **neat-freak** 基于 [数字生命卡兹克](https://github.com/KKKKhazix/Khazix-Skills) 的开源 skill 改编，原项目采用 MIT 许可证。

## 许可证

[MIT](LICENSE)
