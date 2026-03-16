---
name: Vercel Config & Next.js Config
---

# Vercel Config — complete files

---

## `vercel.json` (project root)

```json
{
  "$schema": "https://openapi.vercel.sh/vercel.json",
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
        {
          "key": "Strict-Transport-Security",
          "value": "max-age=63072000; includeSubDomains; preload"
        },
        {
          "key": "X-Content-Type-Options",
          "value": "nosniff"
        },
        {
          "key": "X-Frame-Options",
          "value": "DENY"
        },
        {
          "key": "X-XSS-Protection",
          "value": "1; mode=block"
        },
        {
          "key": "Referrer-Policy",
          "value": "strict-origin-when-cross-origin"
        },
        {
          "key": "Permissions-Policy",
          "value": "camera=(), microphone=(), geolocation=(), interest-cohort=()"
        },
        {
          "key": "Content-Security-Policy",
          "value": "default-src 'self'; script-src 'self' 'unsafe-inline' 'unsafe-eval' https://*.googletagmanager.com https://www.google-analytics.com https://va.vercel-scripts.com https://speed-insights.vercel.com; style-src 'self' 'unsafe-inline' https://fonts.googleapis.com; font-src 'self' https://fonts.gstatic.com; img-src 'self' data: https:; connect-src 'self' https://*.sentry.io https://*.google-analytics.com https://*.googletagmanager.com https://vitals.vercel-insights.com; frame-ancestors 'none';"
        }
      ]
    },
    {
      "source": "/_next/static/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=31536000, immutable"
        }
      ]
    },
    {
      "source": "/images/(.*)",
      "headers": [
        {
          "key": "Cache-Control",
          "value": "public, max-age=86400, stale-while-revalidate=604800"
        }
      ]
    }
  ]
}
```

> ⚠️ The `Content-Security-Policy` header includes inline script allowances (`'unsafe-inline'`) required for GTM. If GTM is removed, tighten this to a nonce-based CSP. Update this header when adding new third-party scripts.

---

## `next.config.ts` (project root)

```ts
import type { NextConfig } from "next";
import { withSentryConfig } from "@sentry/nextjs";

const nextConfig: NextConfig = {
  // Strip console.log/info in production builds
  compiler: {
    removeConsole:
      process.env.NODE_ENV === "production"
        ? { exclude: ["error", "warn"] }
        : false,
  },

  // Image optimisation — add remote sources as needed
  images: {
    remotePatterns: [
      {
        protocol: "https",
        hostname: "cdn.sanity.io",
        pathname: "/images/**",
      },
    ],
    // Vercel serves WebP/AVIF automatically — no formats config needed
  },

  // Redirect trailing slashes (belt-and-braces alongside vercel.json)
  trailingSlash: false,

  // Enable bundle analyser when ANALYZE=true
  ...(process.env.ANALYZE === "true" && {
    // Applied via withBundleAnalyzer wrapper below
  }),

  // Experimental features
  experimental: {
    // Opt in to PPR when stable: partial prerendering
    // ppr: true,
  },
};

// Bundle analyser (only active when ANALYZE=true)
function getConfig() {
  if (process.env.ANALYZE === "true") {
    const withBundleAnalyzer = require("@next/bundle-analyzer")({
      enabled: true,
    });
    return withBundleAnalyzer(nextConfig);
  }
  return nextConfig;
}

export default withSentryConfig(getConfig(), {
  org: process.env.SENTRY_ORG ?? "haide-digital",
  project: process.env.SENTRY_PROJECT ?? "haide-digital-production",
  silent: !process.env.CI,
  widenClientFileUpload: true,
  hideSourceMaps: true,
  disableLogger: true,
});
```

---

## `.env.example` (committed to repo, no real values)

```env
# Site
NEXT_PUBLIC_SITE_URL=

# Analytics
NEXT_PUBLIC_GTM_ID=
NEXT_PUBLIC_GA4_ID=

# Sentry — error monitoring (see error-handling-monitoring skill)
NEXT_PUBLIC_SENTRY_DSN=
SENTRY_DSN=
SENTRY_AUTH_TOKEN=
SENTRY_ORG=
SENTRY_PROJECT=

# Contact form (server-side only — no NEXT_PUBLIC_ prefix)
CONTACT_FORM_EMAIL=

# CMS — Sanity (add when CMS integrated)
SANITY_PROJECT_ID=
SANITY_DATASET=
SANITY_API_TOKEN=

# On-demand revalidation secret (for Sanity webhook trigger)
REVALIDATE_SECRET=
```

> **Never commit `.env.local`.** It is gitignored by default in Next.js. Set preview/production values in Vercel Dashboard → Project → Settings → Environment Variables.

---

## `package.json` scripts

Add these scripts for CI and local use:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "build:analyze": "ANALYZE=true next build",
    "start": "next start",
    "lint": "next lint",
    "type-check": "tsc --noEmit",
    "lhci": "lhci autorun"
  }
}
```
