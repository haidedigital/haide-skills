---
name: component-audit
description: Master pre-ship audit for the Haide Digital website. Runs a 7-domain sequential review — Brand, Architecture, Animation, Mobile, Performance, Copy, SEO — against all other Haide skill standards. Use this skill on every component, section, or page before marking it complete. Output is a scored report with PASS / FAIL / WARNING per section, exact issue descriptions with file:line references, and a concrete fix for every failure. Nothing ships with a FAIL.
---

# Component Audit — Haide Digital

## The Gate

This is the final checkpoint before any component, section, or page ships. It cross-references every Haide Digital skill domain in one pass. Run it in full — do not skip sections.

**Completion rule:**
- ✅ **PASS** or ⚠️ **WARNING** → may ship (warnings are logged, not blocking)
- ❌ **FAIL** → must fix before shipping. No exceptions.

---

## How to Run an Audit

1. Read `references/audit-protocol.md` for the exhaustive per-section questions
2. Read `references/scorecard-template.md` for the output format
3. Work through each section in order — do not skip ahead
4. For each section, state the verdict (`PASS` / `FAIL` / `WARNING`) followed by issues and fixes
5. Issue the final ruling at the bottom

---

## Audit Sequence

### 1 · Brand Audit
**Skill:** `haide-design-system/SKILL.md`

| Check | Verdict |
|---|---|
| Brand colours used from design system tokens only | |
| No hard-coded hex values (`#F15928` → `text-haide-orange`) | |
| Typography: Poppins for headings/CTAs, Noto Sans for body | |
| No `text-sm` (14px) on body copy — minimum `text-base` | |
| No forbidden tone words (`innovative`, `leverage`, `cutting-edge`, etc.) | |
| Visual weight, spacing, and hierarchy feel like Haide Digital | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

### 2 · Architecture Audit
**Skill:** `haide-nextjs-architecture/SKILL.md`

| Check | Verdict |
|---|---|
| Server Component unless hooks/events/browser APIs needed | |
| `"use client"` directive placed at leaf node, not wrapper | |
| TypeScript types explicit — no `any`, no untyped props | |
| File named in `PascalCase.tsx` convention | |
| Placed in correct directory (`/sections`, `/ui`, `/layout`, `/3d`) | |
| Heavy imports (> 30KB): GSAP plugins, Three.js, modals — dynamically imported | |
| No barrel imports from full libraries | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

### 3 · Animation Audit
**Skill:** `gsap-haide-animations/SKILL.md`

| Check | Verdict |
|---|---|
| Animation timing/easing matches Haide motion language | |
| GSAP: `gsap.set()` used for initial states (not CSS `opacity: 0`) | |
| All GSAP contexts / ScrollTrigger instances cleaned up in `useEffect` return | |
| `gsap.matchMedia()` used for responsive animation differences | |
| `prefers-reduced-motion` respected (`useReducedMotion()` or `gsap.matchMedia`) | |
| Only `transform` / `opacity` animated — no layout-triggering properties | |
| `will-change` removed after animation completes | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

### 4 · Mobile Audit
**Skill:** `mobile-first-haide/SKILL.md`

| Check | Verdict |
|---|---|
| Renders correctly at 375px (iPhone SE — smallest supported) | |
| All interactive elements ≥ 44px tap target | |
| No hover-only interactive states — `:active`/`:focus-visible` equivalents present | |
| Body text ≥ 16px at all breakpoints | |
| No horizontal overflow at 375px | |
| Animations simplified on mobile (shorter duration, no scrub) | |
| Three.js scene: static image fallback below 768px | |
| Lenis not active on touch device | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

### 5 · Performance Audit
**Skill:** `performance-budget/SKILL.md`

| Check | Verdict |
|---|---|
| No new render-blocking resources added to `<head>` | |
| All images use `next/image` with `width`, `height`, `sizes` props | |
| LCP image has `priority` prop; all others do not | |
| New JS added to initial bundle is < 30KB (dynamic import if larger) | |
| No CSS `filter` or dimension properties animated | |
| `will-change` lifecycle correct (add before, remove after) | |
| Three.js: geometry, material, texture, renderer all disposed on unmount | |
| No images without explicit aspect ratio (CLS risk) | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

### 6 · Copy Audit
**Skill:** `haide-copy-voice/SKILL.md` (see also `Editorial_Copywriting.md`)

| Check | Verdict |
|---|---|
| All user-facing text reviewed (headings, body, labels, tooltips, error states) | |
| CTAs are specific and action-oriented (not "Click here" or "Learn more" alone) | |
| No passive voice in primary content | |
| No forbidden/overused words (`innovative`, `leverage`, `seamlessly`, etc.) | |
| Tone matches section purpose (bold for hero, precise for services, warm for about) | |
| Numbers formatted correctly (revenue: `£1.2M`, not `1200000`) | |
| All placeholder/lorem ipsum text replaced | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

### 7 · SEO Audit
**Skill:** `haide-seo-schema/SKILL.md`

*(Only required for page-level components and sections that contribute to SEO signals.)*

| Check | Verdict |
|---|---|
| Page has unique `<title>` and `<meta name="description">` | |
| One `<h1>` per page; heading hierarchy correct (H1 → H2 → H3) | |
| All images have descriptive `alt` text (not empty, not "image") | |
| Structured data (JSON-LD) present and valid if applicable | |
| Internal links exist where appropriate (service references, case study links) | |
| No duplicate content or placeholder SEO text | |
| GEO signals present if page targets a location (London, UK) | |

**Verdict:** PASS / FAIL / WARNING
**Issues:**
**Fixes:**

---

## Final Ruling

```
Component: [Name]
File: [path/to/Component.tsx]
Date: [YYYY-MM-DD]

BRAND        ✅ PASS / ⚠️ WARNING / ❌ FAIL
ARCHITECTURE ✅ PASS / ⚠️ WARNING / ❌ FAIL
ANIMATION    ✅ PASS / ⚠️ WARNING / ❌ FAIL
MOBILE       ✅ PASS / ⚠️ WARNING / ❌ FAIL
PERFORMANCE  ✅ PASS / ⚠️ WARNING / ❌ FAIL
COPY         ✅ PASS / ⚠️ WARNING / ❌ FAIL
SEO          ✅ PASS / ⚠️ WARNING / ❌ FAIL

OVERALL: ✅ APPROVED TO SHIP / ❌ REQUIRES FIXES
```

All `❌ FAIL` findings must be resolved and re-audited before the component may be marked complete.

---

## References

- `references/audit-protocol.md` — Exhaustive per-section question list, red flag triggers, and fix guidance for each audit domain
- `references/scorecard-template.md` — Blank scorecard to copy and fill in for each component audit
