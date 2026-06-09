# Agents / Skills 维护清单

本文用于清理 agents、skills、临时脚本和工作区残留时做判断。原则是先确认事实源，再清理镜像、断链和临时产物。

## Skills 事实源

| 路径 | 当前定位 | 清理判断 |
|---|---|---|
| <!-- source-path-1 --> | <!-- description --> | <!-- rule --> |
| <!-- source-path-2 --> | <!-- description --> | <!-- rule --> |

## 清理工作流 Skill

- Skill 名称：`agent-skill-cleanup`
- 用途：清理 agents / skills / 临时脚本 / 备份目录前，先按该 skill 的盘点、隔离、复查和记录流程执行。

## 当前镜像关系

<!-- 根据你的实际 skills 目录结构填写 -->

- <!-- source-dir/skill-a --> 是真实源，<!-- mirror-dir/skill-a --> 中同名目录为 junction。
- <!-- source-dir/skill-b --> 是真实源，<!-- mirror-dir/skill-b --> 中同名目录为 junction。

## 已删除的 Skills（历史记录）

| Skill | 删除日期 | 原因 |
|-------|---------|------|
| <!-- skill-name --> | <!-- date --> | <!-- reason --> |

## 优先清理

1. 断掉的 junction：目标不存在、且没有真实 `SKILL.md` 的目录。
2. 根目录零字节临时文件：明显由一次性任务生成，且不在任何项目规则中引用。
3. `tmp/`、`.tmp_*`、`*_backup_*` 中的临时验证副本：先隔离到 `_cleanup_quarantine/`，确认无误后再删除。
4. `.pytest_cache`、`__pycache__`：可按项目清，但不要把 `.venv` 或数据库文件混在同一批删除。

## 暂不清理

- 运行态或个人数据（数据库、OAuth state、config、raw/、profile/）
- active 项目的 `.venv`、`node_modules`，除非确认可以重新安装
- Agent 系统 skills、插件缓存 skills
- 任何有 `.git` 的项目目录，除非已经确认是废弃克隆或临时 worktree

## 每周建议流程

1. 先跑只读盘点：skills 根目录、断 junction、零字节文件、临时目录体积。
2. 更新本文中的事实源判断，若关系变化先记录再清。
3. 第一批只清断链和明确临时文件。
4. 对较大的备份目录优先移动到 `_cleanup_quarantine/YYYYMMDD/`。
5. 清理后复查 `CLAUDE.md` / `AGENTS.md` 中是否仍引用已移动路径。
