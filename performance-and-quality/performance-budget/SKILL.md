---
name: performance-budget
description: Performance standards and enforcement for the Haide Digital website. Haide is an SEO agency — the site must score 95+ on PageSpeed. Covers Core Web Vitals targets (LCP <1.5s, INP <100ms, CLS <0.05), asset budgets (JS <200KB initial, images WebP/AVIF), Next.js App Router optimisation patterns (RSC-first, dynamic imports, next/font, next/image), animation compositor safety (transform/opacity only, will-change discipline), Three.js disposal, Lighthouse CI automation, and the mandatory performance audit checklist that must pass before any feature ships. Use when building new features, reviewing PRs, debugging slow pages, or setting up monitoring.
---

# Performance Budget — Haide Digital

## The Standard

Haide Digital is an **SEO agency**. A slow site is professional negligence. Target: **Lighthouse 95+** on every page, measured on a simulated mid-tier mobile device (Lighthouse default).

---

## Core Web Vitals Targets

| Metric | Target | Absolute Maximum | Measured by |
|---|---|---|---|
| **LCP** (Largest Contentful Paint) | < 1.2s | 1.5s | Field + Lab |
| **INP** (Interaction to Next Paint) | < 75ms | 100ms | Field data |
| **CLS** (Cumulative Layout Shift) | < 0.03 | 0.05 | Field + Lab |
| **FCP** (First Contentful Paint) | < 0.8s | 1.0s | Lab |
| **TTFB** (Time to First Byte) | < 200ms | 250ms | Cloudflare edge |
| **TBT** (Total Blocking Time) | < 100ms | 150ms | Lab proxy for INP |

> **Note:** INP replaced FID as a Core Web Vital in March 2024. FID targets are no longer tracked.

---

## Asset Budgets

| Asset type | Budget | Hard limit |
|---|---|---|
| JavaScript (initial, gzipped) | < 150KB | 200KB |
| CSS (gzipped, purged) | < 30KB | 50KB |
| Above-fold images | < 200KB total | — |
| Total above-fold page weight | < 400KB | 500KB |
| Font files (total, all subsets) | < 60KB | 80KB |
| Three.js scene JS (lazy) | — | Loaded async, not in budget |

---

## Quick Rules (tattooed to every PR)

**JavaScript:**
- Default to React Server Components — zero client JS cost
- `'use client'` only when hooks or event handlers are required
- Dynamic import anything > 30KB: GSAP plugins, Three.js, heavy UI components
- Never import a whole library for one utility (`import { debounce } from "lodash"` → `import debounce from "lodash/debounce"`)

**Images:**
- WebP for photos, AVIF when broad support is confirmed, SVG for illustrations
- Always `width` + `height` on `<Image>` — prevents CLS
- `priority` prop only on LCP image (hero image, above-fold)
- `sizes` prop on every `<Image>` inside a grid or responsive container

**Fonts:**
- Max 2 font families: Poppins + Noto Sans
- Use `next/font` — zero render-blocking, automatic subsetting, `font-display: swap`
- Never `@import` fonts in CSS (render-blocking)

**Animations:**
- Only animate `transform` and `opacity` — compositor-promoted, no layout recalc
- Never animate: `width`, `height`, `top`, `left`, `padding`, `margin`, `filter`
- `will-change: transform` — add before animation starts, remove when done

---

## Red Flags — Never Ship With These

🚨 **Block the PR** if any of these are present:

| Red flag | Why it's fatal |
|---|---|
| Render-blocking `<script>` or `<link rel="stylesheet">` in `<head>` | Delays FCP/LCP directly |
| `<img>` without explicit `width` and `height` | CLS on image load |
| `will-change: transform` left permanently on static elements | Memory leak, GPU waste |
| CSS `filter` or `mix-blend-mode` animation | Causes expensive composite layers |
| Unused JS > 50KB in initial bundle | TBT / INP regression |
| `import * from "gsap"` or `import * from "three"` | Full library in bundle — never |
| `next/image` with `layout="fill"` but no `sizes` | Downloads wrong image size |
| Font loaded via `<link>` in a client component | Render-blocking |
| Three.js geometry/material not disposed on unmount | Memory leak |
| `position: absolute` image without container `position: relative` + fixed height | CLS |

---

## Performance Audit Checklist

**Run before every feature ship.** Short form — full checklist in `references/audit-checklist.md`.

- [ ] Lighthouse 95+ (mobile, production URL or `.lighthouserc.json` CI)
- [ ] LCP element identified and under 1.5s
- [ ] CLS < 0.05 (check Layout Shift panel in DevTools)
- [ ] No render-blocking resources
- [ ] All `<Image>` have `width`, `height`, `sizes`
- [ ] Initial JS bundle < 200KB (check `.next/analyze/`)
- [ ] Fonts loaded via `next/font`
- [ ] `will-change` removed after animations
- [ ] Three.js: `geometry.dispose()`, `material.dispose()` on unmount

---

## References

- `references/nextjs-patterns.md` — RSC strategy, dynamic imports, `next/image`, `next/font`, `next/script`, ISR, prefetch
- `references/animation-performance.md` — Compositor safety, `will-change` discipline, GSAP `.set()` vs CSS, Three.js disposal, OffscreenCanvas
- `references/asset-budgets.md` — Image format guide, font subsetting, bundle analysis with `@next/bundle-analyzer`, Tailwind CSS purging
- `references/audit-checklist.md` — Full 8-section audit, Lighthouse CI config, DevTools profiling walkthrough, CLS debugging, real-user monitoring
