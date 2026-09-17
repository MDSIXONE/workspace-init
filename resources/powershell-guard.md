# PowerShell execution reference

Consult the relevant item when constructing a fragile PowerShell command or diagnosing a shell error; this is not a per-command checklist.

- PowerShell uses `-eq` / `-ne` for comparison and the backtick for escaping. Single-quoted strings preserve literal `$` characters.
- Command resolution varies by version and user profile. Use `Get-Command` when uncertain; use `curl.exe` when the native curl executable is intended. Do not assume curl or wget are PowerShell aliases.
- Invoke executable paths containing spaces with `&`. Avoid composing file deletion or movement across shells. Resolve destructive targets and verify they remain within the authorized directory.
- Check an external command's exit status before dependent operations. `$ErrorActionPreference = 'Stop'` handles PowerShell errors but is not a universal substitute for checking native exit codes.
- PowerShell pipelines carry objects. Specify text encoding when it matters for file interoperability; diagnose actual encoding failures rather than changing console settings routinely.
