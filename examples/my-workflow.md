# 我的工作流 — 使用示例

> 这是一个示例，展示如何用 agent-workflow-kit 的三个 skill 构建自己的工作流。

## 场景

假设你是一个用 Claude Code 做全栈开发的人，工作区里有 3 个项目：
- `web-app/` — Next.js 前端
- `api-server/` — Express 后端
- `docs/` — 项目文档

你装了这些 skills：
- `mp-review` — 代码审查
- `mp-tdd` — 测试驱动开发
- `mp-diagnose` — 排障诊断
- `humanizer` — 去 AI 味
- 以及 agent-workflow-kit 的三个 skill

## 日常工作流

### 早上开始工作

```
你：我想给 API 加一个用户认证功能，该用什么工具？
Agent：（读 tool-router/REFRENCE.md）→ 代码工程分类 → 推荐 /mp-tdd（先写测试再实现）
```

### 写完代码

```
你：功能写完了，/neat
Agent：（neat-freak 启动）
  → git diff 检查改动
  → 发现新增了 /api/auth 路由
  → 更新 README.md 的 API 列表
  → 更新 docs/architecture.md 的认证流程
  → 更新 CLAUDE.md 的环境变量（新增 JWT_SECRET）
  → 输出变更摘要
```

### 周五下午

```
你：清理一下工作区
Agent：（agent-skill-cleanup 启动）
  → 盘点：发现 2 个断链 junction、5 个零字节文件、1 个 debug 脚本
  → 隔离：旧部署包移到 _cleanup_quarantine/
  → 复查：断链清零
  → 同步：更新 tools-inventory.md（skills 数量变化）
  → 输出清理报告
```

## 配置示例

我的 `workspace-config.yaml`：

```yaml
workspace:
  root: "/Users/me/projects"
  name: "my-dev-workspace"

agent:
  platform: "claude"
  config_file: "CLAUDE.md"
  global_config: "~/.claude/CLAUDE.md"

skills:
  directories:
    - "~/.claude/skills/"
  source_of_truth: "~/.claude/skills/"

docs:
  tools_inventory: "docs/tools-inventory.md"
  agent_maintenance: "docs/agent-maintenance.md"
  memory_dir: "docs/memory/"

cleanup:
  quarantine_dir: "_cleanup_quarantine"
  protected_patterns:
    - ".venv/"
    - "node_modules/"
    - ".git/"
```

## 关键心得

1. **tools-inventory.md 是核心**：三个 skill 都依赖它。花时间填好，后面省很多事。
2. **/neat 要成为习惯**：就像 `git commit` 一样，做完事就跑。不跑 = 文档腐烂。
3. **清理先隔离**：agent-skill-cleanup 默认隔离而不是删除。确认没问题再手动删。
4. **决策树要维护**：装了新工具记得更新 REFERENCE.md，否则 tool-router 找不到它。
