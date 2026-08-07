# workspace-init 技能包索引

技能包基目录：`C:\Users\10478\.agents\skills\workspace-init`（本文件所在目录）。

## 文件结构

| 路径 | 作用 |
| ---- | ---- |
| `SKILL.md` | 技能主文件：初始化流程（含危险目录保护、模板记忆、组件安装逻辑） |
| `config.json` | 项目类型模板记忆：`templates[T] = { common: [...], special: [...] }`，首次使用某类型时自动写入 |
| `INDEX.md` | 本文件：技能包资源索引 |
| `resources/universal-rules.md` | 通用 `AGENTS.md` 模板：`Universal Rules`（三条通用规则）+ `Commit & Pull Request Guidelines`（中文 Emoji 提交规范） |
| `resources/context7-mcp.json` | context7 MCP server 配置模板（`mcp` 段，写入项目级 `.mcp.json`） |
| `resources/project-memory-records/SKILL.md` | 通用 skill `project-memory-records` 的安装源（复制到 `.agents/skills/project-memory-records/SKILL.md`） |

## 触发方式

用户说"初始化AI工作区"、"初始化工作区"、"创建比赛文件夹"、"创建项目文件夹"、"新建工作区文件夹结构"、"AI 工作区初始化" 等初始化文件夹结构的请求时使用本技能。

## 执行流程概要

1. **安全前置检查**：当前目录命中危险目录（`C:\Windows*`、`Program Files*`、`ProgramData`、盘符根目录、用户主目录、`.config`/`.agents` 等配置目录）→ 直接拒绝，提示先切换目录。
2. **初始化前检查**：扫描已存在文件夹，一律保留原内容，只创建不存在的。
3. 读取 `config.json` 模板 → 询问项目类型 → 按 common/special 分类创建文件夹并记忆模板。
4. 创建 `AGENTS.md`（模板：`resources/universal-rules.md`，仅追加缺失小节）。
5. 安装 `project-memory-records`（复制 `resources/project-memory-records/SKILL.md` 到 `.agents/skills/`）。
6. 注册 context7 MCP（`resources/context7-mcp.json` → 项目级 `.mcp.json`，保留现有配置）。
7. 写入 `.workspace-init.json` 标记，汇总输出。

## 维护约定

- 新增资源文件后，同步更新本索引。
- 修改 `SKILL.md` 流程编号时，保持与索引中的概要一致。
