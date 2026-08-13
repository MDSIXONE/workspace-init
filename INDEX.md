# workspace-init 技能包索引

技能包基目录：`C:\Users\10478\.agents\skills\workspace-init`（本文件所在目录）。

## 文件结构

| 路径 | 作用 |
| ---- | ---- |
| `SKILL.md` | 技能主文件：初始化流程（含危险目录保护、模板记忆、组件安装逻辑） |
| `config.json` | 项目类型模板记忆：`templates[T] = { common: [...], special: [...] }`，首次使用某类型时自动写入 |
| `INDEX.md` | 本文件：技能包资源索引 |
| `resources/universal-rules.md` | 通用 `AGENTS.md` 模板：`Universal Rules`（三条通用规则）。提交规范由全局 `github-commit` 技能覆盖，不在模板中重申 |
| `resources/context7-mcp.json` | context7 MCP server 定义模板（`mcp` 段，按客户端约定改写后追加） |
| `resources/clients.json` | 客户端配置约定表：各客户端（opencode/codex/claude code/cursor/vscode 等）的全局配置路径、项目配置文件、格式、`mcp` 键名与环境变量语法，用于查重与改写；可自行维护新增客户端 |
| `resources/project-memory-records/` | 通用 skill `project-memory-records` 的安装源（整个目录复制到 `.agents/skills/project-memory-records/`）。错误档案采用"索引 + 按日期拆分"结构：`MISTAKE_INDEX.md`（主题索引 + 日期索引，小到可一次全读）+ `mistakes/YYYY-MM-DD.md`（按日条目文件，单文件一次读完），避免单一档案无限增长触发读取截断 |
| `resources/project-index/` | 通用 skill `project-index`（项目结构索引）的安装源（整个目录复制到 `.agents/skills/project-index/`）。索引存于技能自带资源 `INDEX.md`：AI 总结项目结构/结构大改后写入（总体结构 + 功能索引表），快速了解项目或定位功能时先读该索引；大项目按模块拆分 `details/`，保持主索引小到可一次全读 |

## 触发方式

用户说"初始化AI工作区"、"初始化工作区"、"创建比赛文件夹"、"创建项目文件夹"、"新建工作区文件夹结构"、"AI 工作区初始化" 等初始化文件夹结构的请求时使用本技能。

## 执行流程概要

1. **安全前置检查**：当前目录命中危险目录（`C:\Windows*`、`Program Files*`、`ProgramData`、盘符根目录、用户主目录、`.config`/`.agents` 等配置目录）→ 直接拒绝，提示先切换目录。
2. **初始化前检查**：扫描已存在文件夹，一律保留原内容，只创建不存在的。
3. 读取 `config.json` 模板 → 询问项目类型 → 按 common/special 分类创建文件夹并记忆模板。
4. 创建 `AGENTS.md`（模板：`resources/universal-rules.md`，仅追加缺失小节）。
5. 安装通用 skills：`project-memory-records`、`github-commit`、`project-lingo`、`project-index`（各自复制 `resources/<名称>/` 整个目录到 `.agents/skills/<名称>/`；`project-lingo` 词条与 `project-index` 索引由各 skill 首次使用时初始化）。
6. 注册 context7 MCP（询问客户端 → 按 `clients.json` 约定查全局配置，已注册则跳过；否则按该客户端约定改写模板并写入项目根配置文件，保留现有配置）。
7. 写入 `.workspace-init.json` 标记，汇总输出。

## 维护约定

- 新增资源文件后，同步更新本索引。
- 修改 `SKILL.md` 流程编号时，保持与索引中的概要一致。
