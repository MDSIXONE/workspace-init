# workspace-init

按项目类型初始化 AI 工作区文件夹结构。

## 功能

- 按"项目类型模板"创建子文件夹（模板记忆保存在 `config.json`，common 通用 / special 特殊）
- 创建通用 `AGENTS.md`（Universal Rules 三条；Windows pwsh 7 环境自动追加 PowerShell 防错规则）
- 安装通用 skill 到 `.agents/skills/`：
  - `project-memory-records`：项目 AI 变更日志 / 错误档案 / 失败方案记录（索引 + 按日分文件）
  - `github-commit`：中文 Emoji Commit & PR 规范
  - `project-lingo`：项目黑话词典
  - `project-index`：项目结构索引（AI 总结项目结构时写入自带资源索引，快速了解项目/定位功能时读取）
- 注册 MCP server `context7`（按客户端约定写入项目级配置并保留现有配置；支持 opencode / codex / claude code / cursor / vscode 等）
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
| `resources/powershell-guard.md` | Windows pwsh 7 防错规则（条件追加） |
| `resources/context7-mcp.json` | context7 MCP 配置模板 |
| `resources/clients.json` | 各 AI 客户端配置约定表 |
| `resources/project-memory-records/` | `project-memory-records` 安装源 |
| `resources/github-commit/` | `github-commit` 安装源 |
| `resources/project-lingo/` | `project-lingo` 安装源 |
| `resources/project-index/` | `project-index` 安装源 |
