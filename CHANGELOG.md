# Changelog

## v0.2.0 — 自动发现

- 去掉所有占位符和手动模板
- 三个 skill 首次运行时自动扫描工作区，生成工具清单、决策树和基线
- 删除 `templates/` 目录（不再需要手动填写）
- tool-router：自动扫描 skills/MCP/CLI/项目，生成决策树
- neat-freak：自动检测 Agent 平台，生成路径速查
- agent-skill-cleanup：自动建立工作区基线，后续对比找差异

## v0.1.0 — 初始版本

- 三个 skill 基础版本（tool-router / neat-freak / agent-skill-cleanup）
- 手动模板和占位符
- MIT 许可证
