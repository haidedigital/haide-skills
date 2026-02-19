---
name: action-catalog
description: Documents every significant action, decision, and implementation step taken during the Haide Digital website build. Designed for case study creation, YouTube content, and internal knowledge transfer. Use at the start of every task session and after every milestone to log what was done, why, and what the outcome was. Produces structured action logs that can be directly converted into case study narratives, video scripts, and process documentation.
---

# Action Catalog — Haide Digital Build Log

## Purpose

Every action we take building haide.digital is content. This skill ensures we capture:

- **What** was done (the technical action)
- **Why** it was done (the strategic reasoning)
- **How** it was done (the process, tools, skills used)
- **What changed** (before/after, metrics, screenshots)
- **Lessons learned** (what worked, what didn't, what we'd do differently)

This feeds directly into: case studies, YouTube build-in-public content, client proposals, and internal SOPs.

---

## When To Log

Log an action entry when ANY of these happen:

| Trigger | Example |
|---|---|
| A new section or component is built | "Built FAQSection from Figma with accordion pattern" |
| A design system decision is made | "Chose #F15928 as primary orange — rejected #FF5B04" |
| A performance improvement is achieved | "Reduced LCP from 3.2s to 1.1s via dynamic imports" |
| A skill is created or updated | "Created mobile-first-haide skill after noticing repeated mobile fixes" |
| A bug is fixed with a non-obvious solution | "Fixed CLS caused by font swap — added size-adjust to @font-face" |
| An architecture decision is made | "Chose server components for sections, client leaves for animations" |
| A tool or integration is set up | "Connected Figma MCP for live design reference in Claude Code" |
| A consolidation or audit is run | "Ran full Skills Audit — found 3 conflicts, 0 full overlaps" |
| Content or copy is written/revised | "Rewrote hero headline from passive to active voice" |
| A milestone is reached | "All 9 home page sections complete and rendering" |

---

## Action Entry Format

Every entry follows this structure. Copy and fill in:

```markdown
## [ACTION-YYYY-MM-DD-NNN] Short Title

**Date:** YYYY-MM-DD
**Phase:** [Build | Polish | Performance | Content | Launch Prep]
**Skills Used:** [list of skills loaded for this task]
**Tools:** [Claude Code, Figma MCP, Notion MCP, GitHub, Vercel, etc.]

### What Was Done
[2-3 sentences describing the concrete action taken]

### Why
[1-2 sentences on the strategic reason — what problem did this solve or what goal did it serve?]

### Process
1. [Step 1]
2. [Step 2]
3. [Step 3]
[Include commands run, files changed, or approaches tried]

### Before / After
- **Before:** [state before the action — metric, screenshot ref, or description]
- **After:** [state after — metric, screenshot ref, or description]

### Files Changed
- `path/to/file.tsx` — [what changed]
- `path/to/other.ts` — [what changed]

### Lessons Learned
- [What worked well]
- [What was unexpected]
- [What we'd do differently next time]

### Content Angles
- **Case study angle:** [how this contributes to a client-facing case study]
- **YouTube angle:** [potential video topic or segment this could feed]
- **Process doc:** [any SOP or checklist this should update]
```

---

## Cataloging Protocol

### At Session Start

1. Note the date, current project phase, and planned tasks
2. List which skills will be loaded
3. Create placeholder entries for expected actions

### During Work

4. Fill in entries as milestones happen — don't batch them at the end
5. Capture exact commands, file paths, and metrics where possible
6. Take note of anything surprising or non-obvious

### At Session End

7. Review and finalize all entries from this session
8. Tag entries with content angles (case study, YouTube, SOP)
9. Update the Notion action log page with new entries
10. Flag any entries that need screenshots or screen recordings

---

## Content Pipeline

Action entries flow into these outputs:

```
Action Entry
  ├── Case Study section (client-facing)
  │     └── Problem → Approach → Result → Metric
  ├── YouTube segment (build-in-public)
  │     └── Hook → Context → Process → Outcome → CTA
  ├── Process SOP (internal)
  │     └── Checklist or decision tree
  └── Skill update (if pattern is reusable)
        └── Add to relevant SKILL.md or create new skill
```

### Case Study Format

When converting entries to case study content:

- **Lead with the business problem** — not the technical action
- **Show the system** — emphasize that this is repeatable, not ad-hoc
- **Include real metrics** — before/after numbers, Lighthouse scores, CWV data
- **Credit the methodology** — "Using our Organic Growth Engineering approach..."

### YouTube Format

When converting entries to video content:

- **Hook** (0-10s): The surprising result or problem statement
- **Context** (10-30s): What we were building and why
- **Process** (30s-3min): Screen recording of the actual work, narrated
- **Outcome** (10-20s): Before/after comparison
- **CTA** (5-10s): Subscribe, check the site, get your growth plan

---

## Tagging System

Tag each entry for easy filtering:

| Tag | Meaning |
|---|---|
| `#build` | New feature or component built |
| `#fix` | Bug fix or issue resolution |
| `#perf` | Performance improvement |
| `#design` | Design decision or brand application |
| `#arch` | Architecture decision |
| `#seo` | SEO implementation or improvement |
| `#content` | Copy, content, or voice work |
| `#tool` | Tool setup, integration, or automation |
| `#skill` | Skill created, updated, or audited |
| `#milestone` | Phase completion or major checkpoint |

---

## Example Entry

```markdown
## [ACTION-2026-02-19-001] Full Skills Audit & Consolidation

**Date:** 2026-02-19
**Phase:** Build
**Skills Used:** All 10 Haide-specific skills reviewed
**Tools:** Claude Code, Notion MCP

### What Was Done
Inventoried all 10 project skills + 7 knowledge base files. Ran cannibalization
analysis across every skill pair. Found 3 conflicts (performance targets, font
loading, file naming), 0 full overlaps, 7 complementary partial overlaps.
Overwrote CLAUDE.md with consolidated master config (16,254 bytes).

### Why
Skills had accumulated organically — needed to ensure no contradictions,
establish clear domain ownership, and create a single source of truth
for every Claude Code session.

### Process
1. Read all .skill ZIPs and SKILL.md directories
2. Read all 7 knowledge base .md files
3. Cross-referenced every skill against every other
4. Documented conflicts with resolutions
5. Built dependency map
6. Wrote new CLAUDE.md from scratch

### Before / After
- **Before:** CLAUDE.md had soft performance targets (LCP < 2.5s), used obsolete
  FID metric, no skill dependency map, no conflict documentation
- **After:** Strict targets (LCP < 1.2s, INP < 75ms), dependency map, 3 conflicts
  documented with resolutions, clean skills registry table

### Files Changed
- `CLAUDE.md` — Complete rewrite (16,254 bytes)

### Lessons Learned
- ZIP-archived .skill files can't be directly read — need extracted directories
- Performance-budget skill had much stricter (correct) targets than CLAUDE.md
- No full overlaps found — each skill was purposefully scoped
- Font loading remains an unresolved migration (documented, not fixed)

### Content Angles
- **Case study:** "How we built a self-auditing AI development system"
- **YouTube:** "I made Claude Code audit its own instructions — here's what it found"
- **Process doc:** Skills Audit checklist (5-phase protocol)
```

---

## References

This skill works alongside all other skills but does not enforce technical rules — it is purely a documentation and content-generation system.
