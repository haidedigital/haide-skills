---
name: Lighthouse CI Configuration
---

# Lighthouse CI — `lighthouserc.js`

Full config for running Lighthouse against the Vercel preview URL on every PR.

---

## `lighthouserc.js`

Place at project root:

```js
/** @type {import('@lhci/cli').LhrConfig} */
module.exports = {
  ci: {
    collect: {
      // URLs are passed via --collect.url in the GitHub Actions command
      // Fallback for local runs:
      url: ["http://localhost:3000", "http://localhost:3000/services", "http://localhost:3000/contact"],
      numberOfRuns: 3,
      settings: {
        // Simulate fast 4G mobile (Lighthouse default — do not change)
        preset: "desktop",
        throttlingMethod: "simulate",
        // Block analytics and tracking to prevent flaky scores
        blockedUrlPatterns: [
          "*.google-analytics.com",
          "*.googletagmanager.com",
          "*.hotjar.com",
          "*.sentry.io",
        ],
      },
    },

    assert: {
      preset: "lighthouse:no-pwa",
      assertions: {
        // Hard minimums — fail PR if below
        "categories:performance":    ["error", { minScore: 0.90 }],
        "categories:accessibility":  ["error", { minScore: 0.90 }],
        "categories:best-practices": ["error", { minScore: 0.90 }],
        "categories:seo":            ["error", { minScore: 0.95 }],

        // Core Web Vitals — hard limits
        "first-contentful-paint":    ["error", { maxNumericValue: 1000 }],  // < 1.0s
        "largest-contentful-paint":  ["error", { maxNumericValue: 1500 }],  // < 1.5s
        "total-blocking-time":       ["error", { maxNumericValue: 150 }],   // < 150ms
        "cumulative-layout-shift":   ["error", { maxNumericValue: 0.05 }],  // < 0.05

        // Warnings — won't block but surface in report
        "speed-index":               ["warn", { maxNumericValue: 2500 }],
        "interactive":               ["warn", { maxNumericValue: 3500 }],

        // Asset rules
        "uses-optimized-images":     ["warn", {}],
        "uses-webp-images":          ["warn", {}],
        "uses-text-compression":     ["error", {}],
        "render-blocking-resources": ["error", {}],
        "unused-javascript":         ["warn", {}],

        // Accessibility must-haves (supplement to axe gate)
        "color-contrast":            ["error", {}],
        "image-alt":                 ["error", {}],
        "label":                     ["error", {}],
        "link-name":                 ["error", {}],
      },
    },

    upload: {
      target: "temporary-public-storage",
      // Or LHCI server if self-hosted:
      // target: "lhci",
      // serverBaseUrl: "https://lhci.haide.digital",
      // token: process.env.LHCI_TOKEN,
    },
  },
};
```

---

## Score Thresholds Reference

| Category | Minimum | Rationale |
|---|---|---|
| Performance | 90 | Haide is an SEO agency — a slow site is professional negligence |
| Accessibility | 90 | WCAG AA requirement; supplements the axe gate |
| Best Practices | 90 | HTTPS, no deprecated APIs, secure asset loading |
| SEO | **95** | Haide's core product — must lead by example |

---

## Local Run

To run Lighthouse CI locally against the dev server:

```bash
# Start dev server in one terminal
npm run dev

# Run LHCI in another
npx lhci autorun --collect.url=http://localhost:3000
```

Or against the Vercel preview URL after a PR is opened:

```bash
npx lhci autorun --collect.url=https://haide-digital-git-feature-xyz.vercel.app
```

---

## Reading the Report

After LHCI runs in GitHub Actions:

1. The **Lighthouse GitHub App** posts a summary comment on the PR (table of scores per URL)
2. A link to the full report on `temporary-public-storage` is included
3. If any assertion fails, the step exits with code 1 → CI fails → merge blocked

Common failure causes:

| Score drops | Common cause |
|---|---|
| Performance | Large JS bundle, render-blocking resource, unoptimised image added |
| Accessibility | Missing `alt`, missing form label, low contrast ratio |
| SEO | Missing `<title>` on a new page, missing `<meta name="description">`, `noindex` accidentally set |
| Best Practices | Mixed content (HTTP asset on HTTPS page), deprecated API used |

---

## Install

```bash
npm install --save-dev @lhci/cli
```

Add to `package.json`:

```json
{
  "scripts": {
    "lhci": "lhci autorun"
  }
}
```
