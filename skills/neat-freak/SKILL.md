---
name: neat-freak
description: 项目收尾时同步文档和记忆，确保知识库始终干净准确。做完事就跑 /neat，像写完代码要 git commit 一样自然。
---

# 洁癖 — Knowledge Base Neat-Freak

> **核心理念**: 做完事 → /neat → 切换/结束
>
> 趁你还记得改了什么，马上同步文档。等到下周再整理，你已经忘了当时为什么这么改了。

## 触发方式

### 显式触发
- `/neat`
- `/sync`
- `同步文档`
- `同步项目记忆`
- `收尾同步`
- `整理项目文档`
- `这个阶段做完了，同步一下`

### 不要触发的情况
- `整理一下`（太泛化）
- `梳理一下`
- `总结一下`
- `帮我理清思路`

只有当用户明确要修改文档、记忆、交接信息时，才触发本 skill。

## 三种模式

| 模式 | 触发方式 | 适用场景 |
|------|---------|---------|
| **dry-run** | `/neat --dry-run` | 只审查，不修改文件 |
| **light** | `/neat` | 日常小步开发收尾（默认） |
| **full** | `/neat --full` | 大功能完成、交接、全量整理 |

## 执行流程

### 第零步：Git 安全检查

修改任何文件之前，先确认工作区状态：

```bash
git status --short
git diff --stat
```

### 第一步：盘点现状

**先 ls，再判断。**

#### light mode 盘点范围
1. 项目根目录：README.md, CLAUDE.md, AGENTS.md, .env.example, package.json 等
2. 与本次改动直接相关的 docs/
3. agent 记忆文件（如有）

#### full mode 盘点范围
1. 所有 agent 记忆文件
2. 项目根目录所有 .md
3. docs/ 下所有 .md
4. 散落在其他目录的 .md

**输出文件清单**（内部用），对每个文件标：「评估过 / 要改 / 不用改」。漏一个不行。

### 第二步：识别变更

用**变更影响矩阵**思考（详见 [references/sync-matrix.md](references/sync-matrix.md)）：

| 本次改动 | 要改的文档 |
|---------|-----------|
| 新增 API/路由 | 项目根 markdown + integration-guide + architecture |
| 新增环境变量 | 项目根 markdown + runbook + integration-guide |
| 新增数据库表 | 项目根 markdown + architecture |
| 新增大特性 | 以上全部 + handoff |
| 跨项目改动 | **上下游两边的 docs 都要改** |

**关键检查**：这次改动是不是跨项目的？如果是，所有依赖项目的文档都要同步。

### 第三步：实际修改

**必须真的用工具修改文件**，"我会怎么改"的描述不算完成。

**修改顺序**：先改 docs/ → 再改 CLAUDE.md/AGENTS.md → 最后理记忆

**编辑原则**：
- **合并优于追加**：新信息更新旧条目，不重复添加
- **删除优于保留**：过期信息比没有信息更糟糕
- **精确优于冗长**：一条记忆说一件事
- **绝对时间**：永远写 `2026-05-27`，不写"今天"
- **面向读者**：想象对方只有 5 分钟能看完
- **受众不混**：CLAUDE.md 不抄 docs/ 全文，docs/ 不写"我记得上次……"

**文档真实性校验**：
- README 的运行命令必须真实存在于 package.json / Makefile 等
- API 路由必须能在代码中找到定义
- 环境变量必须在代码或 .env.example 中使用
- 如果无法确认，不要写成已存在事实

### 第四步：自检清单

改完后逐条检查：

- [ ] 第一步列出的每个文件，都判断了"不用改"或"已改"
- [ ] CLAUDE.md/AGENTS.md 里提到的路径/命令/环境变量在代码中真实存在
- [ ] README 的安装/运行步骤跟代码一致
- [ ] 新增 API：在 integration-guide 和 architecture 都出现了
- [ ] 新增环境变量：在 runbook 和项目根 markdown 都出现了
- [ ] 没有相对时间遗留（"今天"、"昨天"、"最近"）
- [ ] 没有覆盖用户原本未提交的改动
- [ ] dry-run mode 下没有修改任何文件

**哪条打不了勾，回去补。**

### 第五步：输出变更摘要

```markdown
## 同步完成

### 模式
- 本次模式：light / full / dry-run
- 是否修改文件：是 / 否

### Git 状态
- 修改前：简述 git status
- 修改后：简述 git status

### 文档变更
- xxx/CLAUDE.md — xxx
- xxx/README.md — xxx
- xxx/docs/xxx.md — xxx

### 验证
- 已确认命令/路由/环境变量真实存在：xxx

### 未处理
- xxx（原因）
```

## 特殊情况

**项目还没有 README 或 CLAUDE.md**：判断项目是否到了"有可运行代码"的阶段。是 → 创建。还在 vibe 阶段 → 跳过，但摘要里提一句。

**对话没有产生新事实**：审查现有文档有没有过期/冲突/相对时间——审查本身就有价值。

**跨项目改动**：每个项目都要跑一次完整的第一步。不要假设一个项目的 docs 改了，另一个就不用。

## 你的工作区适配

> 根据你的实际项目结构编辑此章节。参考 `references/agent-paths.md` 填写你的 Agent 路径。

### 项目结构
<!-- TODO: 列出你的工作区子项目 -->
- project-a/ — 项目 A 描述
- project-b/ — 项目 B 描述

### Agent 记忆位置
<!-- TODO: 填写你的 Agent 配置和记忆文件位置 -->
- Agent A: CLAUDE.md + docs/memory/
- Agent B: AGENTS.md

### 同步重点
- 新增 Skills → 更新 CLAUDE.md 的可用工具章节
- 新增脚本 → 更新 scripts/ 目录说明
- 跨项目改动 → 检查所有依赖项目的文档

## 参考资料

- **[references/sync-matrix.md](references/sync-matrix.md)** — 变更类型 → 要改哪些文件的完整映射
- **[references/agent-paths.md](references/agent-paths.md)** — 各 Agent 的记忆与配置路径速查
