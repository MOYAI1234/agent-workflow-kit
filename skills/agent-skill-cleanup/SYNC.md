# 工具链清单同步 + 上游更新检查

清理完成后，按本文检查 `tool-router` 相关文档是否需要更新，以及工具上游是否有新版本。

---

## 工具链清单同步

### 触发条件

以下任一情况发生时**必须**执行：

- 删除或新增了 skill（含 junction 断链清理后 skill 数量变化）
- 移动了项目目录
- 新增或删除了独立工具/CLI
- MCP 连接器状态变化（新增/断开/删除）
- 项目归属变化（如 skill 从一个 agent 迁移到另一个）

### 检查清单

| 检查项 | 读取文件 | 判断标准 |
|--------|---------|---------|
| Skills 数量是否一致 | 实际 skills 目录 vs `tools-inventory.md` Skills 章节 | 每个 skill 都有对应条目 |
| 项目目录是否一致 | 实际工作区 vs `tools-inventory.md` 产物目录章节 | 活跃项目都在表中，已删项目已移除 |
| MCP/连接器状态 | 实际 connector 状态 vs `tools-inventory.md` MCP 章节 | 状态标注与实际一致 |
| 决策树分支是否完整 | 本次新增/删除的工具 vs tool-router `REFERENCE.md` | 新工具有入口，删工具已移除 |
| Agent 归属表 | 实际 junction/真实源关系 vs tool-router `REFERENCE.md` | 归属与真实源一致 |

### 更新顺序

1. 先更新 `docs/tools-inventory.md`（完整清单）
2. 再更新 tool-router 的 `REFERENCE.md`（决策树 + 归属表）
3. 两个文件都改完才算完成

### 免检情况

- 仅做了 quarantine 隔离（未实际删除）
- 仅清理了 `__pycache__`、`.pytest_cache` 等缓存
- 仅移动了文件位置但未增删工具/项目

---

## 上游更新检查

每周清理时顺带检查工具链上游是否有新版本。只查**有明确版本号或 commit SHA 的工具**。

### 检查方式

```bash
# CLI 工具
<tool> --version

# GitHub release
gh api repos/<owner>/<repo>/releases/latest

# Python 包
pip show <package>

# npm 包
npm view <package> version
```

### 执行方式

1. CLI 工具：跑 `--version` 或 `gh api` 查最新 release
2. 远端服务：SSH 查 `git log -1 --format=%H`
3. Python 包：`pip show` 或 `gh api`
4. **只报告差异，不自动更新**（更新需要用户确认）

### 输出格式

```markdown
### 上游更新检查

| 工具 | 本地版本 | 上游最新 | 状态 |
|------|---------|---------|------|
| tool-a | 1.2.3 | 1.2.3 | ✅ 最新 |
| tool-b | 1.4.0 | 1.5.0 | ⚠️ 可更新 |
```

### 免检条件

- 上次检查距今 < 7 天（避免重复）
- 工具明确标注 `deprecated` 或 `archived`
