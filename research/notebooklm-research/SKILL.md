---
name: notebooklm-research
description: Manages NotebookLM notebooks via MCP for RAG-powered research. Use when user mentions NotebookLM, notebook research, document queries, or needs to access their notebook library. Triggers on phrases like "ask my notebook", "research in NotebookLM", "check my sources", or "notebook library".
---

# NotebookLM Research Assistant

Conversational research partner using NotebookLM (Gemini 2.5, Session RAG).

## When to Use This Skill

- User wants to query documents/sources in NotebookLM
- User mentions "notebook", "NotebookLM", or "my sources"
- User needs to manage their notebook library
- User asks about research stored in notebooks
- Authentication issues with NotebookLM

## Pre-Flight Checklist

Before any operation, verify readiness:

- [ ] Run `get_health` to check authentication status
- [ ] If `authenticated=false`, guide user through auth setup
- [ ] Confirm active notebook with `list_notebooks` or `select_notebook`

## Core Workflows

### 1. Health Check & Authentication

```
get_health → Check authenticated status
├── authenticated=true  → Ready to proceed
├── authenticated=false → Run setup_auth flow
└── persistent issues   → cleanup_data(preserve_library=true) + setup_auth
```

**Auth Setup Flow:**
1. Run `setup_auth` — opens browser for Google login
2. User has 10 minutes to complete login
3. Verify with `get_health`

**Auth Repair Flow (expired/broken):**
1. Close ALL Chrome/Chromium instances
2. Run `re_auth` for fresh login
3. Verify with `get_health`

**Nuclear Option (persistent failures):**
1. Close ALL browsers
2. `cleanup_data(confirm=true, preserve_library=true)`
3. `setup_auth`

### 2. Research Queries

**Ask questions against notebooks:**
```
ask_question(question, notebook_id?)
```

- If no `notebook_id`, uses active notebook
- No active notebook? Run `list_notebooks` → `select_notebook` first

### 3. Notebook Library Management

| Action | Tool | Permission |
|--------|------|------------|
| View all notebooks | `list_notebooks` | None |
| Get notebook details | `get_notebook(id)` | None |
| Search notebooks | `search_notebooks(query)` | None |
| Library stats | `get_library_stats` | None |
| Set active notebook | `select_notebook(id)` | Announce |
| Update metadata | `update_notebook` | Confirm |
| Add notebook | `add_notebook` | Explicit Yes |
| Remove notebook | `remove_notebook` | Explicit Yes |

### 4. Adding a Notebook (5-Step Conversation)

**Never add without explicit permission. Follow this sequence:**

1. **Ask URL:** "What is the NotebookLM share link?"
2. **Ask content:** "What knowledge is inside?" (1-2 sentences)
3. **Ask topics:** "Which topics does it cover?" (3-5)
4. **Ask use cases:** "When should we consult it?"
5. **Propose & confirm:**
   ```
   Name: [suggested]
   Description: [from user]
   Topics: [list]
   Use cases: [list]
   "Add to library?"
   ```
6. Only on explicit "Yes" → call `add_notebook`

**How user gets share link:**
1. Visit https://notebooklm.google/
2. Click "+ New" → Upload sources
3. Click "Share" → "Anyone with the link"
4. Copy link → Provide to assistant

### 5. Session Management

| Tool | Purpose |
|------|---------|
| `list_sessions` | Show active sessions with stats |
| `close_session(id)` | Close specific session (ask first) |
| `reset_session(id)` | Clear history, keep session ID |

### 6. Auto-Switching Notebooks

**Safe to auto-switch when:**
- Context change is obvious ("Let's work on React now")
- Announce: "Switching to [notebook] for this task..."

**Ask first when:**
- Multiple notebooks could apply
- Ambiguous context

## Dangerous Operations

### remove_notebook

```
User: "Remove the React notebook"
You:  "Remove 'React Best Practices' from your library?
       (Does not delete the actual NotebookLM notebook)"
User: "Yes"
→ call remove_notebook
```

### cleanup_data

**Always preview first:**
```
cleanup_data(confirm=false, preserve_library=true)  → Preview
cleanup_data(confirm=true, preserve_library=true)   → Execute
```

**Categories cleaned:**
1. Legacy Installation (notebooklm-mcp-nodejs)
2. Current Installation (notebooklm-mcp)
3. NPM/NPX Cache
4. Claude CLI MCP Logs
5. Temporary Backups
6. Claude Projects Cache (optional)
7. Editor Logs (optional)
8. Trash Files (optional)

## Quick Reference

### Tool Summary

| Tool | Purpose | Risk |
|------|---------|------|
| `ask_question` | Query notebooks | Safe |
| `list_notebooks` | View library | Safe |
| `get_notebook` | Notebook details | Safe |
| `search_notebooks` | Find notebooks | Safe |
| `get_library_stats` | Usage stats | Safe |
| `select_notebook` | Set active | Low |
| `update_notebook` | Edit metadata | Medium |
| `add_notebook` | Add to library | Medium |
| `list_sessions` | View sessions | Safe |
| `close_session` | End session | Low |
| `reset_session` | Clear history | Medium |
| `get_health` | Check status | Safe |
| `setup_auth` | Initial auth | Low |
| `re_auth` | Re-authenticate | Medium |
| `remove_notebook` | Delete from library | High |
| `cleanup_data` | System cleanup | High |

### Rate Limits (Free Tier)

- 100 notebooks max
- 50 sources per notebook
- 500k words per notebook
- 50 queries per day

*Google AI Pro/Ultra: 5x higher limits*

## Error Handling

**Auth failures:**
1. Check `get_health`
2. Try `re_auth`
3. If persistent: `cleanup_data` + `setup_auth`

**No active notebook:**
1. `list_notebooks`
2. Ask user to select
3. `select_notebook(id)`

**Rate limited:**
1. Inform user of daily limit
2. Suggest `re_auth` to switch accounts
3. Or wait until quota resets
