# 我的工作流 — 使用示例

## 场景

假设你用 Claude Code 做全栈开发，工作区里有 3 个项目，装了几个 skills 和 MCP。

## 第一次使用

```
你：扫描工作区，初始化工具链

Agent：（自动执行）
  → 扫描 ~/.claude/skills/，发现 12 个 skill
  → 扫描 ~/.agents/skills/，发现 8 个 skill（3 个与 Claude 重叠）
  → 扫描 MCP 配置，发现 2 个已连接的 MCP
  → 检查 PATH，发现 gh、git、node、python 等 CLI
  → 扫描工作区，发现 3 个活跃项目
  → 检测到 Claude Code 平台

  → 生成 docs/tools-inventory.md（27 个工具条目）
  → 生成 tool-router/REFERENCE.md（6 个分类的决策树）
  → 生成 neat-freak/references/agent-paths.md
  → 建立工作区基线

  完成。已发现 27 个工具，决策树已生成。
```

## 日常使用

### 找工具

```
你：我想给 API 加一个用户认证功能，该用什么工具？

Agent：（读 REFERENCE.md）
  → 决策树 → 代码工程 → 测试 → 推荐 /mp-tdd
  → 加载 mp-tdd skill 执行
```

### 收尾同步

```
你：/neat

Agent：（neat-freak 启动）
  → git diff 检查改动
  → 发现新增了 /api/auth 路由
  → 更新 README.md 的 API 列表
  → 更新 docs/architecture.md 的认证流程
  → 更新 CLAUDE.md 的环境变量（新增 JWT_SECRET）
  → 输出变更摘要
```

### 定期清理

```
你：清理一下工作区

Agent：（agent-skill-cleanup 启动）
  → 对比基线：发现 2 个新断链、5 个新零字节文件
  → 低风险清理：删除断链和零字节文件
  → 隔离：旧部署包移到 _cleanup_quarantine/
  → 同步：更新 tools-inventory.md
  → 输出清理报告
```

## 安装新工具后

```
你：我刚装了一个新的 MCP 连接器，更新一下工具链

Agent：（tool-router 增量扫描）
  → 只扫描 MCP 配置（跳过已扫描的 skills）
  → 发现新增 1 个 MCP
  → 更新 tools-inventory.md
  → 更新 REFERENCE.md 决策树
  → 完成
```

## 关键心得

1. **首次运行最重要**：花 2 分钟让 agent 扫描完，后面省无数时间
2. **/neat 要成为习惯**：就像 `git commit` 一样，做完事就跑
3. **清理先隔离**：agent-skill-cleanup 默认隔离而不是删除
4. **增量更新**：装了新工具后说"更新工具链"，不用重新扫描全部
