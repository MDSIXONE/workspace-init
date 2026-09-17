---
name: workspace-init
description: Initialize project folders or an AI workspace on request. Folder-only requests create folders; a full AI workspace may include project instructions and selected helper skills. MCP setup is a separate explicit option.
---

# Workspace initialization

## Choose the requested scope

- Folder-only request: create the requested directory structure; do not install skills or configure tools.
- Full AI workspace: create appropriate folders and project instructions, and install the bundled memory, commit, terminology and index helper skills unless the user selects a smaller set. Describe the chosen scope briefly and proceed with clear requests.
- Register Context7 only when requested. Do not infer authorization to modify global client settings from folder creation.

Use supplied project type, names and preferences. Read `config.json` only when reusing or saving a directory template. For new types, infer a modest structure from the request; bundle any material missing choices in one question. Optional folders need not be confirmed individually. Save reusable template changes only when the user requests remembering them.

## Check the target

Resolve the explicit target or current workspace. Refuse accidental initialization of filesystem roots, OS/program directories or the user home itself; ask for a project destination while continuing any independent planning. Do not reject a legitimate project solely because an ancestor has a dot-prefixed name.

Inspect target existence and name collisions, not the contents of every existing directory. Preserve existing files and configuration. Create missing directories only. If a planned file would overwrite user content, merge only an unambiguous, authorized addition; otherwise leave that dependent change pending and explain the conflict.

## Full workspace components

- Consult `resources/universal-rules.md` for minimal project defaults. Read existing project instructions and applicable global rules first; do not copy rules already inherited. Preserve established language, workflow and client conventions.
- Use the actual client's instruction entrypoint. In mixed-client projects, keep shared instructions in one source with explicit references from required entrypoints; do not assume CLAUDE.md overrides AGENTS.md or is automatically read by Codex.
- Install selected helpers from `resources/<skill>/` to `.agents/skills/<skill>/` only when absent. Copy their resources and rename `SKILL.md.template` to `SKILL.md`. Templates use this suffix to prevent accidental global discovery. Do not create empty history, glossary or index files.
- Consult `resources/powershell-guard.md` only for a relevant PowerShell concern. Do not append a shell tutorial to every project's AGENTS.md.

## Optional Context7 setup

Read `resources/context7-mcp.json` and `resources/clients.json` only for requested MCP configuration. Infer the current client from available context. Check whether Context7 is already configured using only relevant fields; avoid printing credentials. Treat the client table as a hint and verify uncertain syntax against current official documentation.

Prefer project-scoped configuration. Preserve unrelated settings, use a format-aware edit for JSON or TOML and validate parsing. If required configuration details or permissions are missing, report them and complete independent workspace steps. Do not refuse an authorized TOML edit merely because it is TOML.

## Completion

Verify requested paths and installed helper entrypoints, plus parsing of any changed configuration. No business-code tests are required for directory scaffolding. For full initialization, record installed components and the project type in `.workspace-init.json`. Summarize created items, preserved conflicts and any pending requested setup. Do not install additional tools, commit or push unless requested.
