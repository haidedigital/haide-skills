---
name: GitHub Actions CI Workflow
---

# Full `.github/workflows/ci.yml`

```yaml
name: CI

on:
  pull_request:
    branches: [main, staging]

concurrency:
  group: ci-${{ github.ref }}
  cancel-in-progress: true

jobs:
  # ─────────────────────────────────────────────────────────────
  # Gate 1: Install, TypeScript, ESLint, Build
  # ─────────────────────────────────────────────────────────────
  build:
    name: Build & Type Check
    runs-on: ubuntu-latest
    outputs:
      preview_url: ${{ steps.vercel_preview.outputs.url }}

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: TypeScript check
        run: npx tsc --noEmit

      - name: ESLint
        run: npm run lint

      - name: Build
        run: npm run build
        env:
          NEXT_PUBLIC_SITE_URL: ${{ secrets.NEXT_PUBLIC_SITE_URL }}
          NEXT_PUBLIC_SENTRY_DSN: ${{ secrets.NEXT_PUBLIC_SENTRY_DSN }}
          SENTRY_DSN: ${{ secrets.SENTRY_DSN }}
          SENTRY_AUTH_TOKEN: ${{ secrets.SENTRY_AUTH_TOKEN }}
          SENTRY_ORG: ${{ secrets.SENTRY_ORG }}
          SENTRY_PROJECT: ${{ secrets.SENTRY_PROJECT }}

      - name: Wait for Vercel preview deployment
        id: vercel_preview
        uses: patrickedqvist/wait-for-vercel-preview@v1.3.2
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          max_timeout: 180
          allow_inactive: false
        env:
          VERCEL_TOKEN: ${{ secrets.VERCEL_TOKEN }}

  # ─────────────────────────────────────────────────────────────
  # Gate 2: Lighthouse CI
  # ─────────────────────────────────────────────────────────────
  lighthouse:
    name: Lighthouse CI
    runs-on: ubuntu-latest
    needs: build

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install LHCI
        run: npm install -g @lhci/cli

      - name: Run Lighthouse CI
        run: |
          lhci autorun \
            --collect.url="${{ needs.build.outputs.preview_url }}" \
            --collect.url="${{ needs.build.outputs.preview_url }}/services" \
            --collect.url="${{ needs.build.outputs.preview_url }}/contact"
        env:
          LHCI_GITHUB_APP_TOKEN: ${{ secrets.LHCI_GITHUB_APP_TOKEN }}

  # ─────────────────────────────────────────────────────────────
  # Gate 3: Bundle Analysis
  # ─────────────────────────────────────────────────────────────
  bundle:
    name: Bundle Size Check
    runs-on: ubuntu-latest
    needs: build

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Analyze bundle
        run: npm run build:analyze
        env:
          ANALYZE: true
          NEXT_PUBLIC_SITE_URL: ${{ secrets.NEXT_PUBLIC_SITE_URL }}

      - name: Check bundle size thresholds
        uses: andresz1/size-limit-action@v1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}

  # ─────────────────────────────────────────────────────────────
  # Gate 4: Security Scan
  # ─────────────────────────────────────────────────────────────
  security:
    name: Security Audit
    runs-on: ubuntu-latest
    needs: build

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Audit dependencies
        run: npm audit --audit-level=high

  # ─────────────────────────────────────────────────────────────
  # Gate 5: Accessibility (axe-core)
  # ─────────────────────────────────────────────────────────────
  accessibility:
    name: Accessibility Check
    runs-on: ubuntu-latest
    needs: build

    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      - name: Run axe accessibility tests
        run: npx playwright test --project=a11y
        env:
          PREVIEW_URL: ${{ needs.build.outputs.preview_url }}

      - name: Upload accessibility report
        if: failure()
        uses: actions/upload-artifact@v4
        with:
          name: accessibility-report
          path: playwright-report/
```

---

## Required GitHub Secrets

Set in: GitHub → Repository → Settings → Secrets and Variables → Actions

| Secret | Description |
|---|---|
| `VERCEL_TOKEN` | Vercel personal access token (for preview URL detection) |
| `LHCI_GITHUB_APP_TOKEN` | Lighthouse CI GitHub App token (posts PR comments) |
| `NEXT_PUBLIC_SITE_URL` | `https://haide.digital` |
| `NEXT_PUBLIC_SENTRY_DSN` | Sentry client DSN |
| `SENTRY_DSN` | Sentry server DSN |
| `SENTRY_AUTH_TOKEN` | Sentry source map upload token |
| `SENTRY_ORG` | `haide-digital` |
| `SENTRY_PROJECT` | `haide-digital-production` |

---

## Branch Protection Setup

GitHub → Repository → Settings → Branches → Add rule for `main`:

- ✅ Require a pull request before merging (1 approval)
- ✅ Require status checks to pass before merging
  - Required checks: `Build & Type Check`, `Lighthouse CI`, `Bundle Size Check`, `Security Audit`, `Accessibility Check`
- ✅ Do not allow bypassing the above settings

---

## Playwright A11y Test Setup

Add to `playwright.config.ts`:

```ts
import { defineConfig, devices } from "@playwright/test";

export default defineConfig({
  projects: [
    {
      name: "a11y",
      use: { ...devices["Desktop Chrome"] },
      testMatch: /.*\.a11y\.spec\.ts/,
    },
  ],
});
```

Add `tests/accessibility.a11y.spec.ts`:

```ts
import { test, expect } from "@playwright/test";
import AxeBuilder from "@axe-core/playwright";

const URLS = ["/", "/services", "/contact"];
const BASE = process.env.PREVIEW_URL ?? "http://localhost:3000";

for (const path of URLS) {
  test(`${path} has no critical a11y violations`, async ({ page }) => {
    await page.goto(`${BASE}${path}`);
    const results = await new AxeBuilder({ page })
      .withTags(["wcag2a", "wcag2aa"])
      .analyze();
    expect(results.violations.filter(v => ["critical", "serious"].includes(v.impact ?? ""))).toEqual([]);
  });
}
```

Install:

```bash
npm install --save-dev @axe-core/playwright @playwright/test
npx playwright install chromium
```
