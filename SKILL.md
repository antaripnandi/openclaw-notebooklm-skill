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
4. Auth is handled via `NOTEBOOKLM_AUTH_JSON` env var.
5. **Self-Healing Auth:** If any `notebooklm` command fails with "Authentication expired" or similar auth error, DO NOT ask the user. Instead, run `python {WORKSPACE_DIR}/skills/notebooklm/scripts/auto_playwright.py` which will automatically launch the browser, snag fresh cookies, and save them. Then retry your `notebooklm` command.
