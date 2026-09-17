# Commit and pull request conventions

## Scope

Distinguish writing a message, committing locally, pushing and opening a PR. Perform only actions included in the user's request or established authorization. A request to write a commit message does not authorize a commit or push.

## Prepare and verify

- Inspect status and the relevant staged/unstaged diff. Stage only this task's intended files or hunks; preserve unrelated changes and existing staging.
- Never commit credentials, secrets or private keys. Respect project hooks and required checks. Reuse valid verification for the same final state; do not rerun unrelated suites merely to commit.
- Follow repository message conventions. Otherwise use a concise subject in the user's language; emoji and a fixed taxonomy are optional. For multiline messages, use a message file with `git commit -F` or a structured tool field, with shell-appropriate quoting.

## Safety and completion

- Do not change global Git configuration implicitly. Explicitly authorized repository configuration changes are allowed.
- Do not perform destructive resets, history rewriting or force pushes without specific authorization. Do not force-push protected main/master branches.
- Do not bypass hooks merely to finish. When a hook fails, inspect whether a commit was created, fix task-related failures and retry the ordinary commit. Amend only if explicitly authorized.
- Verify the resulting commit identifier and intended scope. Verify remote state only when pushing or opening a PR was requested.
- A PR explains the concrete behavior change and relevant validation; include linked issues, risks or screenshots only when applicable. Do not require a generic safety essay for every PR.
