---
name: skill-registry
description: >-
  Post-creation automation for Claude Code skills. Triggers AFTER a skill
  is created or updated via skill-creator. Runs the full deployment
  pipeline: package, push to GitHub, update CLAUDE.md, update Notion,
  and audit the codebase against the new skill. Use when: a new skill
  has just been created, a skill has been updated, or you need to sync
  the skills registry across all systems (GitHub, CLAUDE.md, Notion).
  Triggers on: "register skill", "deploy skill", "sync skills",
  "push skill to GitHub", "update skills registry", "skill created",
  "new skill", after skill-creator completes.
---

# Skill Registry — Post-Creation Pipeline

## Purpose

Every time a skill is created or updated, 8 manual steps must happen before it's truly "done". This skill automates that entire pipeline into a single sequential pass.

**Trigger:** Run this immediately after `skill-creator` finishes, or whenever a skill needs syncing across systems.

---

## The Pipeline

Execute every step in order. Do not skip steps. Mark each step done as you go.

### Step 1: Package the Skill

```
Tool: Bash
Command: PYTHONIOENCODING=utf-8 <python-path> <package-script> <skill-dir> <output-dir>
```

See `references/paths.md` for exact paths. The packager validates YAML frontmatter, directory structure, and file references before creating the `.skill` ZIP.

**If packaging fails:** Fix validation errors and re-run. Do not proceed until the `.skill` file is created.

---

### Step 2: Copy to haide-skills Repo

Determine the correct category directory from `references/category-map.md`.

```
1. Create category dir if it doesn't exist: mkdir -p <haide-skills>/<category>/<skill-name>/
2. Copy SKILL.md and references/ to the target directory
3. Verify the copy: ls <target-dir>
```

---

### Step 3: Update haide-skills README.md

Read `<haide-skills>/README.md`. Find the correct category section. Add a row to the table:

```markdown
| [skill-name](category/skill-name/) | One-line description from SKILL.md frontmatter |
```

If the category section doesn't exist yet, create it in alphabetical order among the existing categories.

---

### Step 4: Commit and Push to GitHub

```bash
cd <haide-skills>
git add <category>/<skill-name>/ README.md
git commit -m "feat: add <skill-name> skill — <short description>"
git push origin main
```

**Never force-push.** If push fails, pull first and resolve conflicts.

---

### Step 5: Update CLAUDE.md

Read the project's `CLAUDE.md`. Make two edits:

**5a. Skills Registry Table** — Add a row in the `### Haide-Specific Skills` table:

```markdown
| <skill-name> | `<skill-name>/SKILL.md` | <load-when description> | ON-DEMAND or ALWAYS |
```

Insert in logical position (group with related skills, not alphabetically).

**5b. Dependency Map** — Add an entry in the dependency map code block:

```
<skill-name>
  └── references: <list skills this skill depends on>
  └── referenced by: <list skills that reference this one>
```

Also update any existing entries that now reference or are referenced by this skill.

---

### Step 6: Update Notion Skills Library

Page ID: See `references/paths.md` → Notion Page IDs → Skills Library

1. Fetch the page to see current content
2. Find the correct category section (see `references/category-map.md` for Notion color mappings)
3. If category exists: add a row to its table
4. If category doesn't exist: insert a new category section with a colored table

Table row format:
```
<tr>
<td>skill-name</td>
<td>Description from SKILL.md frontmatter</td>
<td>Haide Website</td>
</tr>
```

---

### Step 7: Update Notion Future Improvements

Page ID: See `references/paths.md` → Notion Page IDs → Future Improvements

Check the "Missing Skills" table. If this skill was listed as missing:
- Strike through the name: `~~Skill Name~~`
- Update description to match actual skill
- Change status to `CREATED`

If the skill was not in the missing list, skip this step.

---

### Step 8: Audit Codebase Against New Skill

Load the newly created/updated SKILL.md and run it against the current codebase:

1. Read the skill's rules and checklist items
2. Scan the relevant source files for violations
3. Report findings as a summary:
   - Number of issues found
   - Top 3-5 most critical issues with `file:line` references
   - Suggested fixes for each

**Do not auto-fix.** Present findings to the user and let them decide what to action.

If the skill is not code-facing (e.g., process/documentation skills), skip this step and note "N/A — non-code skill".

---

## Verification

After all 8 steps, confirm:

- [ ] `.skill` file exists in project root
- [ ] Skill directory exists in `haide-skills` repo under correct category
- [ ] `haide-skills/README.md` has the new skill listed
- [ ] GitHub commit pushed successfully
- [ ] `CLAUDE.md` registry table includes the skill
- [ ] `CLAUDE.md` dependency map includes the skill
- [ ] Notion Skills Library page has the skill
- [ ] Notion Future Improvements updated (if applicable)
- [ ] Codebase audit results presented (or N/A noted)

---

## Updating an Existing Skill

When a skill is **updated** (not newly created), the pipeline is slightly different:

- **Step 1**: Re-package (overwrites existing `.skill` file)
- **Step 2**: Overwrite files in haide-skills repo (same directory)
- **Step 3**: Update description in README if changed
- **Step 4**: Commit message uses `fix:` or `chore:` prefix instead of `feat:`
- **Step 5**: Update CLAUDE.md only if load-when or dependencies changed
- **Step 6**: Update Notion description only if changed
- **Step 7**: Skip (only for new skills)
- **Step 8**: Re-audit if rules changed

---

## References

- `references/paths.md` — All file paths, tool locations, and Notion page IDs
- `references/category-map.md` — Skill-to-category mapping for repo, README, and Notion
