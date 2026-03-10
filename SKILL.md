---
name: notebooklm
description: Programmatic NotebookLM control with auto-recovery for authentication errors.
---

# NotebookLM CLI

## Commands
```bash
notebooklm list                                    # List all notebooks + IDs
notebooklm create "Name"                           # Create notebook → returns ID
notebooklm source add "<url_or_file>" -n <ID>      # Add URL/PDF/YouTube to notebook
notebooklm source add "<url>" -n <ID> --wait       # Add + wait for processing
notebooklm source list -n <ID>                     # List sources in notebook
notebooklm ask "Question" -n <ID>                  # Query AI against sources
```

## Rules
1. **No playlists.** Extract individual YouTube URLs and add one by one.
2. **Max 50 sources** per notebook.
3. **Use `--wait`** when adding sources in scripts to ensure processing completes.
4. **Dynamic Auth Execution:** The `notebooklm` CLI requires a `NOTEBOOKLM_AUTH_JSON` environment variable to run. **YOU MUST NOT SET THIS PERMANENTLY.** Instead, for every command, read the contents of `~/.notebooklm/auth_payload.json` and inject it dynamically into your execution environment. (e.g., `cross-env NOTEBOOKLM_AUTH_JSON='{...}' notebooklm list` or set it locally in your Node script prior to exec).
5. **Auth Recovery:** If `notebooklm` fails with "Authentication expired", you MUST ask the user for permission. Propose running `python {WORKSPACE_DIR}/skills/notebooklm-bypass/scripts/auto_playwright.py`. If approved, run it. This script steals fresh cookies and saves them to `~/.notebooklm/auth_payload.json`. Once saved, read the new file and retry your command using the Dynamic Auth Execution rule.
