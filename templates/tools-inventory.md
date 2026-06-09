# 工具链盘点手册

> 最后更新：YYYY-MM-DD | 维护者：<!-- your name -->
>
> 本文件是工具链唯一事实源。增删改工具后，同步更新本文件和 tool-router 的 REFERENCE.md。

---

## 一、Agent 概览

| Agent | 入口路径 | 主力场景 | 状态 |
|-------|---------|---------|------|
| <!-- Agent A --> | <!-- path --> | <!-- scenario --> | <!-- active/inactive --> |
| <!-- Agent B --> | <!-- path --> | <!-- scenario --> | <!-- active/inactive --> |

---

## 二、Skills 全量清单

### 2.1 业务 Skills

| Skill | 归属 Agent | 触发场景 | 真实源位置 | 状态 |
|-------|-----------|---------|-----------|------|
| <!-- your-skill-1 --> | <!-- agent --> | <!-- triggers --> | <!-- path --> | <!-- active/inactive --> |
| <!-- your-skill-2 --> | <!-- agent --> | <!-- triggers --> | <!-- path --> | <!-- active/inactive --> |

### 2.2 知识库 Skills

| Skill | 归属 Agent | 用途 | 入口 | 与其他工具对比 |
|-------|-----------|------|------|---------------|
| <!-- kb-1 --> | <!-- agent --> | <!-- purpose --> | <!-- entry point --> | <!-- comparison --> |

**知识库选择决策**：
<!-- TODO: 填写你的知识库选择规则 -->
- <!-- 领域 1 --> → <!-- 工具 1 -->
- <!-- 领域 2 --> → <!-- 工具 2 -->

### 2.3 Connector Skills

<!-- 如果你使用飞书/钉钉/企业微信等平台的 connector skills，在此列出 -->

| Skill | 功能 | 常用场景 |
|-------|------|---------|
| <!-- connector-skill-1 --> | <!-- function --> | <!-- scenario --> |

### 2.4 开发工程 Skills

| Skill | 功能 | 适用场景 |
|-------|------|---------|
| <!-- dev-skill-1 --> | <!-- function --> | <!-- scenario --> |

### 2.5 内容/创作 Skills

| Skill | 功能 | 触发场景 |
|-------|------|---------|
| <!-- content-skill-1 --> | <!-- function --> | <!-- trigger --> |

### 2.6 内置/系统 Skills

| Skill | 归属 | 功能 |
|-------|------|------|
| tool-router | 全 Agent | 工具链决策树 — 做 X 该用什么？ |
| neat-freak | 全 Agent | 项目收尾同步文档和记忆 |
| agent-skill-cleanup | 全 Agent | 清理 agents/skills/临时脚本 |

---

## 三、MCP 插件/连接器清单

| 名称 | 类型 | 状态 | 用途 |
|------|------|------|------|
| <!-- mcp-1 --> | <!-- streamableHttp/command/url --> | <!-- enabled/disabled --> | <!-- purpose --> |

---

## 四、独立工具/CLI

| 工具 | 路径/安装方式 | 功能 | 常用场景 |
|------|-------------|------|---------|
| <!-- tool-1 --> | <!-- path/install --> | <!-- function --> | <!-- scenario --> |

---

## 五、产物/项目目录

| 目录 | 用途 | 关联 Skill |
|------|------|-----------|
| <!-- dir-1 --> | <!-- purpose --> | <!-- skill --> |

---

## 六、Skills 存储拓扑

```
真实源（编辑在这里）          镜像/Junction（不要直接编辑）
─────────────────────        ────────────────────────────
<!-- source-dir/ -->         <!-- mirror-dir/ -->
  ├── skill-a ────────────────→ skill-a (junction)
  └── skill-b ────────────────→ skill-b (junction)
```

**维护规则**：
1. 编辑 skill 时先确认真实源位置，不要改镜像
2. 新增 skill 默认放在 <!-- default-source -->
3. Junction 断掉时不要直接删，先确认真实源是否存在

---

## 七、问题和待处理

| # | 问题 | 影响 | 建议 |
|---|------|------|------|
| <!-- 1 --> | <!-- issue --> | <!-- impact --> | <!-- suggestion --> |
