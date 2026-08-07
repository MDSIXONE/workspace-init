# workspace-init

按项目类型初始化 AI 工作区文件夹结构。

## 功能

- 按"项目类型模板"创建子文件夹（模板记忆保存在 `config.json`，common 通用 / special 特殊）
- 创建通用 `AGENTS.md`（Universal Rules 三条 + Commit & PR Guidelines 中文 Emoji 提交规范）
- 安装通用 skill `project-memory-records` 到 `.agents/skills/`
- 注册 MCP server `context7` 到项目级 `.mcp.json`（保留现有配置）
- **危险目录保护**：当前目录命中系统/程序目录（`C:\Windows*`、`Program Files*`、盘符根目录、用户主目录等）时直接拒绝执行

## 使用

在目标项目目录中向 AI 客户端说：

```
初始化AI工作区 比赛
```

首次使用的项目类型会逐项询问子文件夹并记忆为模板，之后自动复用。

## 文件结构

| 路径 | 作用 |
| ---- | ---- |
| `SKILL.md` | 技能主文件：初始化流程 |
| `config.json` | 项目类型模板记忆 |
| `INDEX.md` | 技能包资源索引 |
| `resources/universal-rules.md` | 通用 `AGENTS.md` 模板 |
| `resources/context7-mcp.json` | context7 MCP 配置模板 |
| `resources/project-memory-records/SKILL.md` | `project-memory-records` 安装源 |
