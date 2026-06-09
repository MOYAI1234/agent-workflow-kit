---
name: agent-skill-cleanup
description: >
  Audit and clean multi-agent workspaces: broken junctions, temp files, stale skills,
  quarantine archives. Use when user says "clean up", "weekly cleanup", "清理 skills",
  "整理工作区", or asks to tidy agent architecture.
---

# Agent / Skill 清理工作流

用于清理多项目、多 agent 工作区。先保住事实源，再清断链、空文件和临时产物。

## 必读入口

开始前先读取：

- 工作区根目录的 `CLAUDE.md` / `AGENTS.md`（了解项目结构）
- `docs/agent-maintenance.md`（了解 skills 镜像关系）
- 任务落到具体项目时，再读该项目的 `AGENTS.md` / `CLAUDE.md`

## 清理流程（7 步）

### 1. 只读盘点
- 统计 skills 目录的顶层目录、`SKILL.md` 数量、junction 关系和断链
- 统计根目录零字节文件、`tmp` / `.tmp_*` / `*_backup*` / 旧部署包
- 统计大体积目录，"大"≠"可删"

### 2. 判断事实源
- 参照 `agent-maintenance.md` 的镜像关系表确认真实源
- mirror entrypoint 不因内容短就删

### 3. 低风险清理
- 删断链 junction（确认 `ReparsePoint=True` 且目标不存在）
- 删零字节、一次性生成、未被引用的临时文件
- Windows 保留名残留（`nul`）用长路径或 .NET 删除

### 4. 散落文件整理
- 调试脚本 `debug_*.py` / `check_*.py` → `scripts/misc/`
- 路径错误文件（文件名含完整路径）→ 删除
- 临时文档 → `docs/reports/`
- 截图（非项目资源）→ 删除
- 核心配置文件（`CLAUDE.md`、`AGENTS.md`、`package.json`）不动

### 5. 隔离而不是硬删
- 大目录/历史备份/旧部署包 → `_cleanup_quarantine/YYYYMMDD-<label>`
- 隔离前解析绝对路径，确认在工作区内

### 6. 复查和记录
- 复查断 junction = 0，被删/隔离路径已消失
- `rg` 检查 live docs 是否还引用旧路径
- 写入 `docs/migration-log.md`

### 7. 工具链同步
- 清理可能改变 skills 数量、项目结构、工具归属
- 详见 [SYNC.md](SYNC.md)

## 红线（摘要）

- 不删事实源、mirror entrypoint、运行态数据（数据库、OAuth state、config）
- 不把 `.venv`、`node_modules` 和临时文件混同批清理
- **所有删除/移动/覆盖操作必须用户确认**
- 完整红线和注意事项 → [REFERENCE.md](REFERENCE.md)

## 建议输出

最终回复包含：清理结果、隔离清单、复查证据、散落文件整理清单。

完整输出模板（含 Skills 评估、Workspace Map、记忆文件、工具链同步结果）→ [REFERENCE.md](REFERENCE.md)
