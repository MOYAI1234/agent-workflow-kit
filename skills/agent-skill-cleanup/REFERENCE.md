# Agent / Skill 清理 — 详细参考

本文是 `SKILL.md` 的补充材料。日常清理只需读 SKILL.md，需要详细规则时查阅本文。

---

## 红线（完整）

- 不删除 active skill 的事实源
- 不删除 mirror entrypoint，除非用户明确确认不再需要
- 不删除运行态数据（数据库文件、OAuth state、`raw/`、`profile/`、`config/settings.yaml` 等）
- 不把 `.venv`、`node_modules` 和临时文件混在同一批清理；依赖缓存要单独确认
- 不在 PowerShell 中用超长单行或复杂 Bash 写法串联删除命令
- **所有涉及删除、移动、覆盖的操作，必须先获得用户明确确认**

## 注意事项

### Windows / PowerShell

- 删除或移动前用 `Resolve-Path` 校验绝对路径
- 递归删除或移动前确认路径在预期工作区内
- junction 只删除链接本身，不跟随删除目标
- 遇到 `ParserError`、`Unrecognized token`、`invalid format`，优先怀疑 PowerShell 参数解析
- 使用 `ForEach-Object`、`Where-Object` 全名，避免 `%` / `?` 在嵌套命令里漂移

### macOS / Linux

- 删除前用 `readlink -f` 确认符号链接目标
- 用 `find -type l` 查找断链符号链接
- 避免在 `~/.config`、`~/Library` 等系统目录做批量操作

---

## Skills 评估流程

每周清理时可选执行：

1. **分类盘点** — 第三方（有外部 GitHub/marketplace 来源）vs 自建（用户业务场景）
2. **第三方 skills 检查** — 读 SKILL.md 的 version/homepage，访问 GitHub 查最新 release，对比本地版本
3. **自建 skills 优化** — 检查 description 是否覆盖常见触发词、SKILL.md 中路径/常量是否与当前环境一致
4. **环境适配评估** — 标记仅适用单一 agent 平台、已废弃、或与其他 skill 功能重复的 skill

---

## Workspace Map 更新

1. 对比实际目录与 `docs/workspace-overview.md` 和 `docs/project-index.md`
2. 识别未记录的活跃项目（有 `AGENTS.md` 或 `README.md`）
3. 识别探索/历史目录，分类到"暂不维护"
4. 更新文档，记录变更到 `docs/migration-log.md`

---

## 记忆文件检查与优化

### 记忆文件审计

1. 检查 `docs/memory/` 下的记忆文件是否过时
2. 检查各项目 `AGENTS.md` / `CLAUDE.md` 中的路径、运行方式、部署信息是否与实际一致
3. 发现不一致时标记，由用户确认后更新

### 工作区级文档优化

1. **子项目列表更新** — 检查 `CLAUDE.md` 中的子项目列表是否包含所有活跃项目（有 `AGENTS.md`/`README.md` 且 30 天内有更新）
2. **目录结构描述更新** — 新增目录需补充说明，已删除目录需移除
3. **AGENTS.md 同步** — 镜像副本确保内容同步

### 项目级文档补全

检查活跃项目是否有 `AGENTS.md`，缺失时根据 `README.md` 创建基础模板：

```markdown
# <项目名>

<一句话描述>

## 核心路径
| 目录 | 用途 |

## 工作原则
- <关键规则>

## 相关项目
- `<workspace>/<related>` - <说明>
```

### 用户级文档同步

- 不同 agent 的全局配置应包含相同核心规则
- 通用规则（Shell 约定、SSH 约定、记忆同步约定）：双写
- 客户端专属规则：只写在对应文件

---

## 建议输出（完整模板）

最终回复包含：

- 新增或更新的维护文档
- 删除了哪些断链或空文件
- 哪些目录只是隔离，隔离位置和大小
- 哪些高风险项刻意没有动
- 复查证据：断链数量、路径是否消失、是否仍有 live 引用
- Skills 评估报告：第三方更新状态、自建优化建议、环境适配评估
- Workspace Map 变更记录
- 记忆文件检查与优化结果
- 散落文件整理清单：归档了哪些、删除了哪些
- 工具链清单同步结果：`tools-inventory.md` 和 `REFERENCE.md`（tool-router）是否已更新
