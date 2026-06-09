---
name: tool-router
description: "工具链决策树 — 做 X 该用什么工具？查询/维护全量工具清单。当用户说「用什么工具」「该用什么」「怎么做 X」「哪个 skill」「工具清单」或在多个同类工具间犹豫不决时使用。"
---

# Tool Router — 工具链决策树

> 完整清单：`docs/tools-inventory.md`（路径可在 `workspace-config.yaml` 中配置）
> 决策树详情：[REFERENCE.md](REFERENCE.md)

## Quick start

用户说「我想做 X」→ 读 REFERENCE.md 中对应分类的决策树 → 定位工具 → 如需详情读 `tools-inventory.md` → 执行。

## Workflows

### 1. 用户不知道用什么工具

```
1. 读 REFERENCE.md，按用户意图匹配分类
2. 在决策树中定位具体工具
3. 如有多个候选，按对比表推荐最优
4. 如需详情，读 tools-inventory.md 对应条目
5. 执行用户任务
```

### 2. 用户要增删改工具

```
1. 增加：安装 → 编辑 tools-inventory.md → 编辑 REFERENCE.md 决策树
2. 删除：确认无依赖 → 从 tools-inventory.md 移除 → 从 REFERENCE.md 移除 → 清理目录
3. 修改：更新 tools-inventory.md → 更新 REFERENCE.md
4. 同类工具超 2 个时，必须在 tools-inventory.md 写对比表
```

### 3. 用户要查工具冲突

```
1. 读 tools-inventory.md 的「问题和待处理」章节
2. 读 REFERENCE.md 的「Agent 归属速查」表
3. 指出重叠和独占关系
```

## 维护规则

- 增加新工具：安装 → 编辑 `tools-inventory.md` → 编辑 `REFERENCE.md`
- 删除工具：确认无依赖 → 从两个文件移除 → 清理目录
- 同类工具超 2 个时，必须在 `tools-inventory.md` 写对比表

## Advanced features

- 决策树详情、Agent 归属表、维护指南 → [REFERENCE.md](REFERENCE.md)
- 完整清单（含 MCP/连接器/产物目录）→ `docs/tools-inventory.md`
