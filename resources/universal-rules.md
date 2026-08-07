# Universal Rules

- Think in English and answer in Chinese unless the context requires otherwise.
- Do not write defensive or fallback code; it does not solve the root problem. Prefer full exposure: let failures surface clearly (explicit errors, exceptions, logs, failing tests) so bugs are visible and can be fixed at the root cause.
- When editing existing code: if you notice unrelated dead code, mention it - don't delete it.

## Commit & Pull Request Guidelines

Use a Chinese Emoji subject in the form `Emoji 范围：简短动作`, for example `🧭 导航：优化目标点路径规划`. Use one coherent change per commit. Pull requests should explain affected sections, safety implications, validation, linked issues, and layout screenshots when needed.

### 提交格式

```
Emoji 类型[范围]：简短动作描述

[可选正文]

[可选脚注]
```

主题行形式：`Emoji 范围：简短动作`，例如：

```
🧭 导航：优化目标点路径规划
✨ 界面：新增深色模式切换
🐛 登录：修复令牌过期后跳转异常
```

### 类型对照表（Emoji + 中文 + 语义）

| Emoji | 中文类型 | 英文等价 | 用途 |
| ----- | -------- | -------- | ---- |
| ✨ | 功能 | `feat` | 新功能 |
| 🐛 | 修复 | `fix` | Bug 修复 |
| 📚 | 文档 | `docs` | 仅文档变更 |
| 🎨 | 样式 | `style` | 格式化/代码风格（无逻辑变更） |
| ♻️ | 重构 | `refactor` | 代码重构（非功能/修复） |
| ⚡ | 性能 | `perf` | 性能优化 |
| ✅ | 测试 | `test` | 新增/更新测试 |
| 📦 | 构建 | `build` | 构建系统/依赖变更 |
| 🚀 | 部署 | `ci` | CI/配置变更 |
| 🧹 | 杂务 | `chore` | 维护/杂项 |
| ⏪ | 回滚 | `revert` | 回滚提交 |

范围（scope）用中文或短模块名，如 `导航`、`认证`、`db`。

### 破坏性变更

```
# 类型后加感叹号
✨!: 移除已废弃的接口

# 或使用 BREAKING CHANGE 脚注
✨ 配置：允许配置扩展其他配置

BREAKING CHANGE: `extends` 键行为已变更
```

### 提交工作流

1. 分析 diff：已暂存用 `git diff --staged`，未暂存用 `git diff`，同时 `git status --porcelain`。
2. 按需暂存（`git add` 或 `git add -p` 分组）。**绝不提交机密**（.env、credentials.json、私钥等）。
3. 从 diff 判定类型、范围、描述（一般现在时、祈使语气、≤72 字符）。
4. 执行提交：`git commit -m "✨ 界面：新增深色模式切换"`；多行用 heredoc 带正文/脚注（`Closes #123`、`Refs #456`）。

### Git 安全协议

- 绝不修改 git 配置。
- 未经明确要求绝不执行破坏性命令（--force、hard reset）。
- 未经用户要求绝不跳过 hooks（--no-verify）。
- 绝不强制推送到 main/master。
- 提交因 hooks 失败时，修复后创建**新提交**（不 amend）。
