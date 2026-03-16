---
name: cicd-deployment
description: CI/CD pipeline, deployment process, and quality gates for Haide Digital on Vercel. Nothing ships to production without passing these gates. Covers branch strategy (main/staging/feature/*), GitHub Actions workflow (build → Lighthouse CI → bundle analysis → security scan → a11y check), vercel.json configuration, environment variable management, Vercel-specific optimisations (Edge Runtime, ISR, analytics, Speed Insights), deployment checklist, and rollback procedure. Use when setting up the pipeline, troubleshooting a failing check, preparing for production deploy, or onboarding a new developer.
---

# CI/CD & Deployment — Haide Digital

## The Standard

Nothing merges to `main` without passing every gate. A failed Lighthouse score, a high-severity dependency vulnerability, or a TypeScript error is a merge blocker — not a warning.

---

## 1 · Stack

| Layer | Tool |
|---|---|
| Hosting | Vercel (haide.digital) |
| Framework | Next.js 14 — native Vercel support, no adapter |
| CI runner | GitHub Actions |
| Preview deployments | Vercel built-in (auto per PR) |
| Monitoring | Sentry, Vercel Analytics, Vercel Speed Insights, GA4 |

---

## 2 · Branch Strategy

| Branch | Maps to | Auto-deploys to |
|---|---|---|
| `main` | Production | `haide.digital` |
| `staging` | Pre-production review | `staging.haide.digital` |
| `feature/*` | Active development | Vercel preview URL (auto per PR) |

**Rules:**
- Never commit directly to `main`
- Always open a PR from `feature/*` → `staging`, or `staging` → `main`
- Branch protection on `main`: require ≥ 1 PR review + all CI checks passing
- Delete merged feature branches

---

## 3 · GitHub Actions Pipeline

All checks run on every PR. A single failure blocks merge.

**Workflow file:** `.github/workflows/ci.yml` (see `references/github-actions.md` for full file)

### Gate 1 — Install & Build

```
npm ci
npx tsc --noEmit          # TypeScript: zero errors permitted
npm run lint              # ESLint: zero errors permitted
npm run build             # Full Next.js production build
```

Fail conditions:
- Any TypeScript error → fail
- Any ESLint error (not warning) → fail
- Build exits non-zero → fail

### Gate 2 — Lighthouse CI

Runs against the Vercel preview URL. Config: `lighthouserc.js` (see `references/lighthouse-ci.md`).

| Category | Minimum score |
|---|---|
| Performance | **90** |
| Accessibility | **90** |
| Best Practices | **90** |
| SEO | **95** |

- Fail PR if any score falls below threshold
- Post scores as a PR comment (table format)
- Run on 3 URLs: `/`, `/services`, `/contact` — all must pass

### Gate 3 — Bundle Analysis

```bash
ANALYZE=true npm run build
```

Post a PR comment flagging:
- Any route's initial JS > **200 KB** (gzipped)
- Total above-fold page weight > **500 KB**

Not a hard block by default — flag and require acknowledgement. Make it a hard block if > 300 KB. Config: see `references/bundle-analysis.md`.

### Gate 4 — Security Scan

```bash
npm audit --audit-level=high
```

- Fail on any **high** or **critical** severity vulnerability
- `moderate` and below: log, do not fail

### Gate 5 — Accessibility Check

```bash
npx playwright test --project=a11y
```

Or with `axe-core` CLI against the Vercel preview URL:

```bash
npx axe-core https://<VERCEL_PREVIEW_URL> --exit
```

- Fail on any **critical** or **serious** WCAG AA violations
- Post violation list as PR comment
- Aligns with `accessibility-a11y` skill requirements

---

## 4 · `vercel.json`

Place at project root. See `references/vercel-config.md` for full file.

```json
{
  "cleanUrls": true,
  "trailingSlash": false,
  "redirects": [
    {
      "source": "/:path*",
      "has": [{ "type": "host", "value": "www.haide.digital" }],
      "destination": "https://haide.digital/:path*",
      "permanent": true
    }
  ],
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "X-Content-Type-Options",       "value": "nosniff" },
        { "key": "X-Frame-Options",              "value": "DENY" },
        { "key": "X-XSS-Protection",             "value": "1; mode=block" },
        { "key": "Referrer-Policy",              "value": "strict-origin-when-cross-origin" },
        { "key": "Permissions-Policy",           "value": "camera=(), microphone=(), geolocation=()" },
        { "key": "Strict-Transport-Security",    "value": "max-age=63072000; includeSubDomains; preload" },
        {
          "key": "Content-Security-Policy",
          "value": "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://*.googletagmanager.com https://www.googleanalytics.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; img-src 'self' data: https:; connect-src 'self' https://*.sentry.io https://*.google-analytics.com;"
        }
      ]
    }
  ]
}
```

---

## 5 · Vercel Project Settings

| Setting | Value |
|---|---|
| Framework preset | Next.js (auto-detected) |
| Node.js version | **20.x** |
| Build command | `next build` (Vercel default — do not override) |
| Output directory | `.next` (Vercel default) |
| Install command | `npm ci` |
| Root directory | `.` (project root) |

Set in: Vercel Dashboard → Project → Settings → General.

---

## 6 · Environment Variables

**Never commit real values.** The `.env.example` file is committed; `.env.local` is gitignored.

| Variable | Environment | Purpose |
|---|---|---|
| `NEXT_PUBLIC_SITE_URL` | All | Canonical site URL (`https://haide.digital`) |
| `NEXT_PUBLIC_GTM_ID` | All | Google Tag Manager container ID |
| `NEXT_PUBLIC_GA4_ID` | All | Google Analytics 4 Measurement ID |
| `NEXT_PUBLIC_SENTRY_DSN` | All | Sentry client-side DSN |
| `SENTRY_DSN` | All | Sentry server-side DSN |
| `SENTRY_AUTH_TOKEN` | CI + Production | Source map upload token |
| `SENTRY_ORG` | CI | Sentry organisation slug |
| `SENTRY_PROJECT` | CI | Sentry project slug |
| `CONTACT_FORM_EMAIL` | Production + Preview | Destination for contact form submissions (server-side only) |
| `SANITY_PROJECT_ID` | All | Sanity CMS project ID (add when CMS integrated) |
| `SANITY_DATASET` | All | Sanity dataset (`production` / `staging`) |
| `SANITY_API_TOKEN` | Production | Sanity read-write API token (server-side only) |

Set in Vercel Dashboard → Project → Settings → Environment Variables. Scope each variable to the appropriate environments (Production / Preview / Development).

---

## 7 · Vercel-Specific Optimisations

### Edge Runtime

Any API route that does not require Node.js APIs (file system, native modules, etc.) should opt in to the Edge Runtime for lower cold-start latency:

```ts
// app/api/contact/route.ts
export const runtime = "edge";
```

Do not use Edge Runtime for routes that require: `fs`, `crypto`, `child_process`, or server-side Sentry heavy operations.

### Static Generation

Marketing pages must be statically generated. Do not use `dynamic = "force-dynamic"` on pages that have no per-request data.

```ts
// app/page.tsx — default is static, nothing to add
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = await getAllPosts();
  return posts.map((p) => ({ slug: p.slug }));
}
```

### ISR (Incremental Static Regeneration)

For pages with CMS content (when Sanity is integrated):

```ts
export const revalidate = 3600; // Re-generate at most once per hour
```

For on-demand revalidation via Sanity webhook:

```ts
// app/api/revalidate/route.ts
import { revalidatePath } from "next/cache";
import { NextResponse } from "next/server";

export async function POST(request: Request) {
  const secret = request.headers.get("x-revalidate-secret");
  if (secret !== process.env.REVALIDATE_SECRET) {
    return NextResponse.json({ error: "Unauthorized" }, { status: 401 });
  }
  const { path } = await request.json();
  revalidatePath(path);
  return NextResponse.json({ revalidated: true });
}
```

### Image Optimisation

Vercel runs `next/image` optimisation by default — no extra config needed. For external image sources, add patterns to `next.config.ts`:

```ts
const nextConfig = {
  images: {
    remotePatterns: [
      { protocol: "https", hostname: "cdn.sanity.io" },
      { protocol: "https", hostname: "images.unsplash.com" },
    ],
  },
};
```

### Vercel Analytics & Speed Insights

Install once, renders real-field Core Web Vitals data in the Vercel dashboard:

```bash
npm install @vercel/analytics @vercel/speed-insights
```

Add to `app/layout.tsx`:

```tsx
import { Analytics } from "@vercel/analytics/react";
import { SpeedInsights } from "@vercel/speed-insights/next";

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        {children}
        <Analytics />
        <SpeedInsights />
      </body>
    </html>
  );
}
```

Review weekly in Vercel Dashboard → Analytics and Speed Insights tabs.

---

## 8 · Domain Configuration

| Domain | Record type | Value |
|---|---|---|
| `haide.digital` | A | `76.76.21.21` (Vercel) |
| `www.haide.digital` | CNAME | `cname.vercel-dns.com` |
| `staging.haide.digital` | CNAME | `cname.vercel-dns.com` |

- Add all three in Vercel Dashboard → Project → Settings → Domains
- Set `haide.digital` as primary (non-www)
- The `www` → non-www redirect is handled in `vercel.json` (see Section 4)
- SSL is provisioned automatically by Vercel via Let's Encrypt

**Branch alias for staging:**

Vercel Dashboard → Domains → Add → enter `staging.haide.digital` → select branch `staging`.

---

## 9 · Deployment Checklist

### Pre-Deploy

- [ ] All GitHub Actions checks passing on the PR
- [ ] Lighthouse scores ≥ thresholds on the Vercel preview URL
- [ ] Preview URL manually tested on mobile (375px) and desktop (1440px)
- [ ] No `console.error` or unhandled errors in browser DevTools on preview
- [ ] Analytics events firing on preview (GTM Preview mode active)
- [ ] Contact form submitting correctly on preview
- [ ] All internal links return 200 (use `next-sitemap` or manual spot-check)
- [ ] Sentry source maps uploading in CI logs (no 401/403 errors)
- [ ] `.env.example` updated if new variables were added

### Post-Deploy

- [ ] Verify `haide.digital` loads the new deployment within 60 seconds
- [ ] Check Sentry → Issues for new errors in the first 30 minutes
- [ ] Verify GA4 receiving live data (Realtime report)
- [ ] Run PageSpeed Insights on `https://haide.digital` — confirm ≥ 90 performance
- [ ] Verify Vercel Analytics showing data in dashboard
- [ ] Check Google Search Console for any new coverage issues (24h delay expected)

---

## 10 · Rollback Procedure

**Policy: roll back first, investigate second. Never debug on production.**

1. Vercel Dashboard → Project → **Deployments** tab
2. Find the last known-good deployment
3. Click ⋯ → **Promote to Production**
4. Vercel instantly routes production traffic to that build — DNS is unchanged
5. Confirm `haide.digital` is serving the correct build (check version/date in footer if present)
6. Open a `hotfix/*` branch to investigate and fix the issue
7. Re-deploy via standard PR flow

Rollback takes < 30 seconds. There is no need to touch DNS or Cloudflare settings.

---

## Verification Checklist

Run once when setting up the project; revisit after significant infrastructure changes.

- [ ] `.github/workflows/ci.yml` exists and all 5 gates are configured
- [ ] `lighthouserc.js` exists with correct thresholds and URLs
- [ ] `vercel.json` exists with security headers, www redirect, `cleanUrls`, `trailingSlash`
- [ ] `.env.example` in repo root documents all required variables (no real values)
- [ ] Vercel project connected to GitHub repo with auto-deploy enabled
- [ ] Branch protection rule on `main`: require PR + all status checks passing
- [ ] `staging.haide.digital` domain alias configured in Vercel
- [ ] `@vercel/analytics` and `@vercel/speed-insights` installed and in `layout.tsx`
- [ ] Sentry releases configured with git SHA tagging
- [ ] Rollback procedure tested at least once in a non-production context

---

## References

- `references/github-actions.md` — Full `.github/workflows/ci.yml` with all 5 gates, Vercel preview URL extraction, PR comment posting
- `references/lighthouse-ci.md` — `lighthouserc.js` config, multi-URL setup, score thresholds, CI integration
- `references/vercel-config.md` — Complete `vercel.json`, `next.config.ts` with Sentry and bundle analyser, `.env.example`
- `references/bundle-analysis.md` — `@next/bundle-analyzer` setup, interpreting results, flagging oversized routes
