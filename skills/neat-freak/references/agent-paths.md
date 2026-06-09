# Agent 路径速查

各 Agent 的记忆与配置文件位置。根据你实际使用的平台填写。

## Claude Code

| 用途 | 路径 |
|------|------|
| 全局配置 | `~/.claude/CLAUDE.md` |
| 项目配置 | `<project>/CLAUDE.md` |
| 项目记忆 | `~/.claude/projects/<project-hash>/memory/` |
| Skills 目录 | `~/.claude/skills/` 或 `<project>/skills/` |

## OpenAI Codex

| 用途 | 路径 |
|------|------|
| 全局配置 | `~/.codex/AGENTS.md` |
| 项目配置 | `<project>/AGENTS.md` |
| Skills 目录 | `~/.agents/skills/` |

## WorkBuddy

| 用途 | 路径 |
|------|------|
| 全局配置 | `~/.workbuddy/CLAUDE.md` |
| 项目配置 | `<project>/CLAUDE.md` |
| Skills 目录 | `~/.workbuddy/skills/` 或 `<project>/.workbuddy/skills/` |
| 记忆文件 | `~/.workbuddy/MEMORY.md` |
| 工作区记忆 | `<project>/.workbuddy/memory/` |

## Trae / SOLO

| 用途 | 路径 |
|------|------|
| 项目配置 | `<project>/CLAUDE.md` 或 `<project>/AGENTS.md` |
| Skills 目录 | `<project>/skills/` 或 `~/.agents/skills/` |

## OpenClaw

| 用途 | 路径 |
|------|------|
| 全局配置 | `~/.openclaw/CLAUDE.md` |
| Skills 目录 | `~/.openclaw/skills/` |

## 你的工作区

<!-- TODO: 填写你的工作区项目 -->

| 项目 | 主配置文件 | 备注 |
|------|-----------|------|
| <!-- workspace root --> | CLAUDE.md + AGENTS.md | <!-- notes --> |
| <!-- project a --> | <!-- config --> | <!-- notes --> |
| <!-- project b --> | <!-- config --> | <!-- notes --> |

## 同步策略

1. **优先同步项目根配置**：CLAUDE.md / AGENTS.md
2. **记忆系统可选**：如果平台支持，同步到记忆目录
3. **docs/ 必须同步**：这是给其他人看的
4. **双写保持一致**：如果同时有 CLAUDE.md 和 AGENTS.md，内容要一致
