# Agent Workflow Kit

一套可配置的 AI 编码 Agent 工作流管理 skills 集合。帮你解决三个问题：

1. **做 X 该用什么工具？** — `tool-router` 工具链决策树
2. **做完事怎么同步？** — `neat-freak` 收尾文档同步
3. **定期怎么清理？** — `agent-skill-cleanup` 工作区深度清洁

三个 skill 独立可用，组合使用效果最好：做事时路由工具 → 做完同步文档 → 定期清理工作区。

## 谁需要这个

- 用 Claude Code / Codex / WorkBuddy 等 AI 编码 Agent 的人
- 工作区里有多个项目、多个 skills、多个 agent 平台的人
- 受够了"这个工具在哪"、"文档没同步"、"断链满天飞"的人

## 快速开始

### 1. 安装 skills

把 `skills/` 下的三个目录复制到你的 agent skills 目录：

```bash
# Claude Code
cp -r skills/tool-router ~/.claude/skills/
cp -r skills/neat-freak ~/.claude/skills/
cp -r skills/agent-skill-cleanup ~/.claude/skills/

# Codex
cp -r skills/tool-router ~/.agents/skills/
cp -r skills/neat-freak ~/.agents/skills/
cp -r skills/agent-skill-cleanup ~/.agents/skills/
```

### 2. 配置你的工作区

复制配置模板并按你的环境填写：

```bash
cp templates/workspace-config.yaml ./
cp templates/tools-inventory.md docs/
cp templates/agent-maintenance.md docs/
```

编辑 `workspace-config.yaml`，填入你的工作区路径、agent 类型、skills 目录等。

### 3. 填写工具清单

编辑 `docs/tools-inventory.md`，把你实际在用的工具、skills、MCP 连接器填进去。这是三个 skill 共享的核心资产。

### 4. 开始用

```
# 不知道用什么工具？
"我想做数据报表，该用什么工具？"  → tool-router 会帮你定位

# 做完事要同步？
/neat                              → neat-freak 同步文档和记忆

# 定期清理？
"清理一下工作区"                   → agent-skill-cleanup 安全清理
```

## 目录结构

```
agent-workflow-kit/
├── README.md                          # 本文件
├── LICENSE                            # MIT
├── skills/
│   ├── tool-router/                   # 工具链决策树
│   │   ├── SKILL.md                   # 入口
│   │   └── REFERENCE.md              # 决策树详情（模板）
│   ├── neat-freak/                    # 收尾文档同步
│   │   ├── SKILL.md                   # 入口
│   │   └── references/
│   │       ├── agent-paths.md         # Agent 路径速查（模板）
│   │       └── sync-matrix.md         # 变更影响矩阵
│   └── agent-skill-cleanup/           # 工作区清理
│       ├── SKILL.md                   # 入口
│       ├── REFERENCE.md              # 详细规则
│       └── SYNC.md                   # 工具链同步指南
├── templates/
│   ├── workspace-config.yaml          # 工作区配置模板
│   ├── tools-inventory.md             # 工具清单模板
│   └── agent-maintenance.md           # Agent 维护清单模板
└── examples/
    └── my-workflow.md                 # 自定义工作流示例
```

## 三个 Skill 的关系

```
做事时                    做完时                   定期时
──────                    ──────                   ──────
tool-router              neat-freak              agent-skill-cleanup
"用什么工具？"            "同步文档"               "清理工作区"
     │                        │                        │
     └────────────────────────┼────────────────────────┘
                              │
                    共享：tools-inventory.md
                    （你的工具清单 = 唯一事实源）
```

**tool-router** 负责发现和选择工具。它读取你的工具清单，按决策树帮你定位"做 X 该用什么"。

**neat-freak** 负责收尾同步。每次做完事跑 `/neat`，自动检查哪些文档需要更新、哪些记忆需要清理。

**agent-skill-cleanup** 负责深度清洁。每周或定期跑一次，清理断链 junction、临时文件、过期 skills。

## 自定义指南

### 添加新工具到决策树

1. 在 `docs/tools-inventory.md` 添加条目
2. 在 `skills/tool-router/REFERENCE.md` 的对应分类下添加分支
3. 如果是新分类，加一个新的顶级节点

### 适配你的 Agent 平台

不同 Agent 平台的配置文件位置不同。编辑 `skills/neat-freak/references/agent-paths.md`，填入你实际使用的平台路径。

支持的平台：Claude Code、OpenAI Codex、WorkBuddy、Trae、OpenClaw 等。

### 自定义清理红线

编辑 `skills/agent-skill-cleanup/REFERENCE.md` 的红线章节，添加你工作区特有的保护规则（比如某些目录绝对不能删）。

## 设计原则

- **唯一事实源**：工具清单（tools-inventory.md）是三个 skill 共享的核心，改一处全生效
- **安全优先**：所有删除/移动操作必须用户确认，隔离优先于硬删
- **平台无关**：不绑定特定 Agent 平台，通过配置适配
- **渐进式**：三个 skill 各自独立，可以只装一个，也可以全装

## 许可证

[MIT](LICENSE)
