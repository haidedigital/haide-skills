# Paths & IDs

All file paths, tool locations, and external IDs needed by the skill-registry pipeline.

---

## Local Paths

| Item | Path |
|---|---|
| Project root | `d:/Work/Vibe Code/Antigravity/Haide Website - Figma Import` |
| haide-skills repo | `d:/Work/Vibe Code/Antigravity/haide-skills` |
| CLAUDE.md | `d:/Work/Vibe Code/Antigravity/Haide Website - Figma Import/CLAUDE.md` |

## Tools

| Tool | Path / Command |
|---|---|
| Python | `/c/Users/Admin/AppData/Local/Programs/Python/Python313/python.exe` |
| package_skill.py | `d:/Work/Vibe Code/Antigravity/haide.digital website/Skills/skill-creator/scripts/package_skill.py` |
| Package command | `PYTHONIOENCODING=utf-8 <python> <package_script> <skill-dir> <output-dir>` |

## GitHub

| Item | Value |
|---|---|
| Repo | `haidedigital/haide-skills` |
| Branch | `main` |
| Remote | `origin` |

## Notion Page IDs

| Page | ID |
|---|---|
| Skills Library | `30c396e4-bf57-81cf-b448-f5facc530b09` |
| Future Improvements | `30c396e4-bf57-8194-b6f9-fc08de7acc06` |
| Haide Website (parent) | `2fa396e4bf578133a8a5ff7358e60c91` |

## Notion Fetch Pattern

```
1. Use mcp__claude_ai_Notion__notion-fetch with id: "<page-id>"
2. Use mcp__claude_ai_Notion__notion-update-page for edits
3. For selection_with_ellipsis: use ~20 chars start + ... + ~20 chars end
4. If selection fails with "Multiple matches", use longer/more unique snippets
5. If insert_content_after fails, fall back to replace_content_range with broader selection
```

## Git Commit Conventions

| Action | Prefix | Example |
|---|---|---|
| New skill | `feat:` | `feat: add testing skill (Vitest, Playwright)` |
| Updated skill | `fix:` or `chore:` | `chore: update component-audit to 10-domain review` |
| Multiple skills | `feat:` | `feat: add security-headers and analytics-tracking skills` |

Always append:
```
Co-Authored-By: Claude Opus 4.6 <noreply@anthropic.com>
```
