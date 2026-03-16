---
name: Bundle Analysis
---

# Bundle Analysis — Haide Digital

Keep the initial JavaScript payload lean. Reference from the `performance-budget` skill:

| Asset | Budget | Hard limit |
|---|---|---|
| Initial JS per route (gzipped) | < 150 KB | **200 KB** |
| Total above-fold page weight | < 400 KB | **500 KB** |

---

## Setup

### 1 · Install

```bash
npm install --save-dev @next/bundle-analyzer
```

### 2 · `next.config.ts` integration

Already included in `references/vercel-config.md`. The analyser wraps the Next.js config when `ANALYZE=true`:

```ts
const withBundleAnalyzer = require("@next/bundle-analyzer")({ enabled: true });
return withBundleAnalyzer(nextConfig);
```

### 3 · Run

```bash
ANALYZE=true npm run build
# or
npm run build:analyze
```

This opens two browser tabs automatically:
- **Client bundle**: `http://localhost:8888` — the bundle served to the browser
- **Server bundle**: `http://localhost:8889` — server-side code (RSC, API routes)

> The client bundle tab is the primary concern for performance.

---

## Reading the Treemap

### Finding oversized routes

Each coloured rectangle = a JS module. Size proportional to parsed (uncompressed) size.

1. Look for unexpectedly large rectangles
2. Common culprits: `node_modules` blocks inside the initial bundle (should be in `async chunks`)
3. Anything that should be lazy-loaded but appears in the initial entry point

### The entry point check

After building, check `.next/analyze/`:
- `client.html` — interactive treemap
- Check the "Entry Points" section — each route's initial JS should be < 200 KB gzipped

Command-line check without the visual treemap:

```bash
# List all page JS bundle sizes (approximate, uncompressed)
find .next/static/chunks -name "*.js" | xargs du -sk | sort -rn | head -20
```

---

## `size-limit` Integration (Gate 3 in CI)

[`size-limit`](https://github.com/ai/size-limit) enforces byte budgets as a CI check and posts a diff comment on PRs.

### Install

```bash
npm install --save-dev size-limit @size-limit/preset-app
```

### `package.json` config

```json
{
  "size-limit": [
    {
      "path": ".next/static/chunks/pages/index-*.js",
      "name": "Homepage JS",
      "limit": "200 kB",
      "gzip": true
    },
    {
      "path": ".next/static/chunks/pages/services-*.js",
      "name": "Services page JS",
      "limit": "200 kB",
      "gzip": true
    },
    {
      "path": ".next/static/chunks/pages/contact-*.js",
      "name": "Contact page JS",
      "limit": "200 kB",
      "gzip": true
    }
  ],
  "scripts": {
    "size": "size-limit"
  }
}
```

Run locally:

```bash
npm run build && npm run size
```

Output:

```
  Package              Size      Limit    Status
─────────────────────────────────────────────────
  Homepage JS         87 kB    200 kB    ✅ Passed
  Services page JS   112 kB    200 kB    ✅ Passed
  Contact page JS     94 kB    200 kB    ✅ Passed
```

---

## Common Fixes for Oversized Bundles

| Problem | Fix |
|---|---|
| GSAP fully imported | `import gsap from "gsap"` only; import plugins separately: `import ScrollTrigger from "gsap/ScrollTrigger"` |
| Three.js in initial bundle | `dynamic(() => import("@/components/3d/Scene"), { ssr: false })` |
| Lucide icons — all icons | `import { ArrowRight } from "lucide-react"` (tree-shakes fine) — never `import * from "lucide-react"` |
| date-fns full library | `import { format } from "date-fns"` (already tree-shakes) |
| Lodash | `import debounce from "lodash/debounce"` — never `import _ from "lodash"` |
| A component is `"use client"` unnecessarily | Remove the directive — moves component to RSC, zero client JS cost |
| Heavy library for a single use | Replace with a lighter alternative or one-off implementation |

---

## Interpreting CI Output

In the GitHub Actions Gate 3 step, `andresz1/size-limit-action` posts a comment like:

```
📦 Bundle Size Report

| Page          | Size   | Change    | Status   |
|---------------|--------|-----------|----------|
| Homepage JS   | 87 KB  | +2 KB     | ✅ Pass  |
| Services JS   | 112 KB | +45 KB ⚠️ | ⚠️ Flag  |
| Contact JS    | 94 KB  | +1 KB     | ✅ Pass  |
```

If any page exceeds the 200 KB hard limit, the CI step exits with code 1 → merge blocked.
