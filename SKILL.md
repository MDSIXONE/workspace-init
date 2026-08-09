---
name: workspace-init
description: 按项目类型初始化 AI 工作区文件夹结构，创建通用 AGENTS.md（含三条通用规则），安装 project-memory-records 与 github-commit 通用 skill 并注册 context7 MCP，使用技能记忆模板（通用文件夹自动创建，特殊文件夹逐项询问）。Use when 用户说"初始化AI工作区"、"初始化工作区"、"创建比赛文件夹"、"创建项目文件夹"、"新建工作区文件夹结构"、"AI 工作区初始化" 等初始化文件夹结构的请求。
---

# 工作区初始化 (Workspace Init)

在**当前工作目录**下完成五件事：

1. 按"项目类型模板"创建子文件夹（模板记忆保存在同目录 `config.json`）。
2. 创建通用 `AGENTS.md`（含三条通用规则）。
3. 安装通用 skill `project-memory-records` 到 `.agents/skills/`。
4. 安装通用 skill `github-commit` 到 `.agents/skills/`。
5. 注册 MCP server `context7`（配置见 `resources/context7-mcp.json`）。

## 数据文件 config.json

```json
{
  "templates": {
    "比赛": {
      "common": ["比赛规则", "比赛源码", "文档", "临时文件"],
      "special": ["比赛地图", "机器信息", "仿真环境", "技术报告"]
    }
  }
}
```

- `common`：通用文件夹，每次自动创建。
- `special`：特殊文件夹，每次逐项询问是否需要。
- 模板可以按项目类型（比赛、研发、课程作业等）各存一套；`config.json` 不存在时视为空配置。

## 流程

0. **安全前置检查（危险目录保护）**：先解析当前工作目录的绝对路径（Windows 下不区分大小写），若命中以下任一情况，**立即停止**，不做任何创建/写入/复制操作，并告知用户当前目录是系统/危险目录，请先切换到真正的项目目录（如 `cd` 到用户自己的工作区）后再调用本技能：
   - Windows 系统目录及其子目录：`C:\Windows`（含 `System32`、`SysWOW64`、`WinSxS` 等）、`C:\Windows.old`；
   - 程序与系统数据目录：`C:\Program Files`、`C:\Program Files (x86)`、`C:\ProgramData`；
   - 文件系统根目录（如 `C:\`、`D:\`）；
   - 用户主目录本身（如 `C:\Users\用户名`）以及工具/配置目录（路径中含 `.config`、`.agents`、`.claude`、`.opencode` 等以 `.` 开头的配置目录）；
   - 其他明显属于操作系统或已安装软件（如 `C:\Windows\System32` 内的任何子路径）的目录。
   只有确认当前目录是用户自己的项目/工作区目录（如 `C:\Users\用户名\projects\xxx`）后才继续后续步骤。
1. **初始化前检查**：先扫描当前工作目录，列出已存在的文件夹及其内部内容。对每个已存在的文件夹：
   - **完整保留原文件夹及内部所有文件/子文件夹**，不删除、不清空、不重建、不覆盖任何内容；
   - 仅在文件夹不存在时才创建新文件夹；
   - 将检查结果记入汇总（"已存在（保留原内容）"），并告知用户哪些文件夹已存在且被原样保留。
2. 读取 `config.json`，加载已有模板。
3. 询问用户：这个文件夹用来做什么项目/用途（如"比赛"、"研发"、"课程作业"）。得到类型名 `T`（若用户直接说"初始化AI工作区 比赛"这类带类型的话，跳过询问）。
4. 若 `templates` 中**没有**类型 `T`（首次使用该类型）：
   a. 询问需要创建哪些子文件夹，让用户列出（逗号/空格/换行分隔均可）。可给常用建议：比赛规则、比赛地图、比赛源码、机器信息、仿真环境、文档、临时文件、技术报告。
   b. 创建用户列出的所有文件夹（仅创建不存在的；已存在且有内容的文件夹保留原内容，跳过创建）。
   c. 将列出的文件夹逐一询问分类：通用（每次都要）还是特殊（下次按需询问）。默认建议：规则/源码/文档/临时文件→通用；地图/环境/报告/机器信息→特殊，用户可修改。
   d. 写入 `config.json`：`templates[T] = { "common": [...], "special": [...] }`，并告知用户模板已记忆。
5. 若类型 `T` 已有模板：
   a. 直接创建全部 `common` 文件夹（已存在则跳过并保留原内容）。
   b. 逐个询问 `special` 文件夹是否需要创建（已存在且非空的保留原内容，无需重建）。
   c. 询问是否要追加新文件夹或调整分类；用户补充的文件夹默认加入 `special`（可再改），随后更新 `config.json`。
6. **创建通用 AGENTS.md**（仅当不存在时创建；若已存在，则仅追加缺失的小节，不覆盖已有内容）：模板内容见资源文件 `resources/universal-rules.md`（Base directory 为本 skill 目录），用该模板创建/追加 AGENTS.md。模板含 `Universal Rules`（三条通用规则）。提交规范见 `github-commit` 技能（全局已注册、自动加载），无需在 AGENTS.md 中重申。
7. **安装通用 skill project-memory-records**：若 `.agents/skills/project-memory-records/SKILL.md` 不存在，把本技能内置的 `resources/project-memory-records/` 整个目录原样复制到 `.agents/skills/project-memory-records/`，保持子目录结构（当前仅含 `SKILL.md`；后续新增资源文件时一并复制）。
8. **安装通用 skill github-commit**：若 `.agents/skills/github-commit/SKILL.md` 不存在，把本技能内置的 `resources/github-commit/` 整个目录（含 `SKILL.md` 与 `resources/commit-guidelines.md`）原样复制到 `.agents/skills/github-commit/`，保持子目录结构。
9. **注册 MCP server context7**：读取资源文件 `resources/context7-mcp.json`（Base directory 为本 skill 目录，server 定义模板）与 `resources/clients.json`（Base directory 为本 skill 目录，各客户端配置约定表），按以下顺序处理：
   - **确认客户端**：询问用户当前使用的 AI 客户端（如 opencode、codex、claude code、cursor、vscode 等）。`clients.json` 已记录常见客户端的全局配置路径、项目级配置文件名、格式与 `mcp` 键名；未记录的客户端，请用户提供其全局/项目级配置位置与格式；
   - **先检查全局是否已注册**：按 `clients.json` 中该客户端的 `globalConfig` 路径逐一读取全局配置，若 `context7` 已注册，**跳过写入**，汇总中记录"全局已注册，无需项目级配置"，本步结束；
   - **写入项目根**：目标配置文件一律写入**项目根**（用 `git rev-parse --show-toplevel` 判定，未检出 git 时用工作区根），而非当前子目录；文件名、`mcp` 键名（如 opencode 的 `mcp`、claude code/cursor 的 `mcpServers`、vscode 的 `servers`）、环境变量语法（如 `{env:VAR}`、`${VAR}`）均按 `clients.json` 中该客户端的约定，将模板中的 server 定义改写后追加；
   - JSON 格式配置（opencode.json、.mcp.json、.cursor/mcp.json、.vscode/mcp.json 等）：若目标文件不存在，创建之；若已存在，保留全部现有字段，仅在客户端对应的 `mcp` 键对象中追加 `context7`，**不覆盖、不删除现有配置**；
   - TOML 等其他格式（如 codex 的 config.toml）：不自动改写，给出该客户端的 context7 手动配置示例（含 mcp 键名与语法）供用户粘贴；
   - 环境变量引用语法因客户端而异（如 `${VAR}`、`{env:VAR}`、直接字面值等）：优先用 `clients.json` 记录的语法，未记录的默认 `${VAR}` 并询问用户。
10. 在当前目录写入标记文件 `.workspace-init.json`（记录项目类型、已创建文件夹、已装组件、时间），用于重复初始化时跳过已存在目录。
11. 汇总输出：已创建、已存在（保留原内容）、模板记忆与组件安装情况。

## 说明

- 文件夹名使用用户原话（允许中文），创建在当前工作目录下。
- **安全保护**：流程 0 必须先于一切操作执行；当前工作目录命中危险目录（Windows 系统目录、程序目录、根目录、用户主目录/工具配置目录等）时，直接拒绝并提示用户切换目录，绝不继续。
- **初始化前必须检查目标文件夹**：已存在的文件夹一律跳过创建，且完整保留其原有内容（含已有文件与子文件夹），不删除、不清空、不覆盖任何内容。
- 汇总输出中需明确区分：新创建、已存在（保留原内容）、模板记忆与组件安装情况。
- 每次使用结束，若 `config.json` 有更新，主动告知用户记忆变化，便于下次复用。
