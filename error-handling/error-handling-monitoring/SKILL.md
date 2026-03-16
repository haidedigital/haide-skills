---
name: error-handling-monitoring
description: Production error handling and monitoring standard for Haide Digital. A broken page for a potential client is a lost lead. Covers the full Next.js error boundary hierarchy (error.tsx, not-found.tsx, loading.tsx), component-level error boundaries for Three.js and GSAP, Suspense placement rules, form error resilience, API route error contracts, Sentry configuration and alert thresholds, logging conventions, and ChunkLoadError recovery. Use when building any page, route, or interactive component — and when setting up Sentry or CI monitoring. Output is a PASS / FAIL verdict against the verification checklist.
---

# Error Handling & Monitoring — Haide Digital

## The Standard

A potential client hitting a blank screen or a stock Next.js error page is a lost lead. Every error state must be handled, branded, and monitored. This skill is non-negotiable before production launch.

---

## 1 · Next.js Error Hierarchy

Next.js App Router has three built-in error surface files. All three must exist and be fully branded.

### `/app/error.tsx` — Global Error Boundary

Catches unhandled errors in any page or layout below `app/layout.tsx`. This is the 500-equivalent.

**Rules:**
- Must be a `"use client"` component (React error boundaries are class-based under the hood)
- Displays branded Haide error UI — never a blank screen or the default Next.js error page
- Never exposes stack traces or technical details to the user
- Logs the error to Sentry via `captureException`
- Provides a clear recovery path: "Go home" and optionally "Try again"

```tsx
// app/error.tsx
"use client";

import { useEffect } from "react";
import Link from "next/link";
import * as Sentry from "@sentry/nextjs";

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  useEffect(() => {
    Sentry.captureException(error);
  }, [error]);

  return (
    <html lang="en">
      <body className="min-h-screen bg-[#16232A] flex items-center justify-center px-6">
        <div className="text-center max-w-lg">
          <p className="font-poppins font-semibold text-haide-orange text-sm uppercase tracking-widest mb-4">
            Something went wrong
          </p>
          <h1 className="font-poppins font-bold text-white text-4xl md:text-5xl leading-tight mb-6">
            We hit a snag.
          </h1>
          <p className="font-noto text-white/60 text-base leading-relaxed mb-10">
            An unexpected error occurred. Our team has been notified. Try refreshing the page or head back to the homepage.
          </p>
          <div className="flex flex-col sm:flex-row gap-4 justify-center">
            <button
              onClick={reset}
              className="bg-haide-orange text-white font-poppins font-semibold px-8 py-4 rounded-full hover:brightness-110 transition-all min-h-[48px]"
            >
              Try again
            </button>
            <Link
              href="/"
              className="bg-white/10 text-white font-poppins font-semibold px-8 py-4 rounded-full hover:bg-white/20 transition-all min-h-[48px] flex items-center justify-center"
            >
              Go home
            </Link>
          </div>
        </div>
      </body>
    </html>
  );
}
```

---

### `/app/not-found.tsx` — 404 Page

**Rules:**
- Fully branded — Haide design system (Poppins, Noto Sans, haide-orange, dark navy)
- Returns actual HTTP 404 status (guaranteed by Next.js when placed at `app/not-found.tsx`)
- Three navigation options: Homepage, Services, Contact
- No technical jargon — friendly, on-brand copy
- Optional: animated orange arrow mark (respects `prefers-reduced-motion`)

```tsx
// app/not-found.tsx
import Link from "next/link";

export default function NotFound() {
  return (
    <main className="min-h-screen bg-[#FDFAF7] flex items-center justify-center px-6">
      <div className="text-center max-w-xl">
        <p className="font-poppins font-semibold text-haide-orange text-sm uppercase tracking-widest mb-4">
          404
        </p>
        <h1 className="font-poppins font-bold text-[#16232A] text-5xl md:text-6xl leading-tight mb-6">
          Page not found.
        </h1>
        <p className="font-noto text-[#4B5563] text-lg leading-relaxed mb-10">
          This page doesn't exist (yet). Head back to somewhere useful.
        </p>
        <div className="flex flex-col sm:flex-row gap-4 justify-center">
          <Link href="/" className="bg-haide-orange text-white font-poppins font-semibold px-8 py-4 rounded-full hover:brightness-110 transition-all min-h-[48px] flex items-center justify-center">
            Go home
          </Link>
          <Link href="/contact" className="bg-[#16232A] text-white font-poppins font-semibold px-8 py-4 rounded-full hover:bg-black transition-all min-h-[48px] flex items-center justify-center">
            Contact us
          </Link>
        </div>
      </div>
    </main>
  );
}
```

---

### `/app/loading.tsx` — Loading Skeletons

**Rules:**
- Never show a lone spinner — show content-shaped skeleton matching the actual layout
- Skeleton base color: `#E4EEF0` (Haide Sand), animated shimmer
- Skeleton shape must approximate the real content (card shapes for card sections)
- Works automatically as Suspense fallback for the root segment

```tsx
// app/loading.tsx
export default function Loading() {
  return (
    <div className="min-h-screen bg-[#FDFAF7] pt-24 px-4" aria-busy="true" aria-label="Loading page content">
      {/* Hero skeleton */}
      <div className="max-w-[1560px] mx-auto">
        <div className="max-w-3xl mb-8">
          <div className="skeleton h-6 w-32 rounded-full mb-6" />
          <div className="skeleton h-16 w-full rounded-2xl mb-3" />
          <div className="skeleton h-16 w-4/5 rounded-2xl mb-6" />
          <div className="skeleton h-5 w-full rounded-lg mb-2" />
          <div className="skeleton h-5 w-3/4 rounded-lg" />
        </div>
      </div>
    </div>
  );
}
```

Shimmer animation in `globals.css`:

```css
.skeleton {
  background: #E4EEF0;
  background-image: linear-gradient(
    90deg,
    #E4EEF0 0%,
    #D5E5E9 40%,
    #E4EEF0 80%
  );
  background-size: 400% 100%;
  animation: shimmer 1.4s ease infinite;
}

@keyframes shimmer {
  0% { background-position: 100% 0; }
  100% { background-position: -100% 0; }
}

@media (prefers-reduced-motion: reduce) {
  .skeleton {
    animation: none;
    background-image: none;
  }
}
```

---

## 2 · Suspense Boundaries

**Rules:**
- Wrap every `async` Server Component in `<Suspense>` with a skeleton fallback
- Wrap every `dynamic()` import in `<Suspense>`
- Never let the whole page suspend — use granular boundaries at the section level
- The root `loading.tsx` is a last resort; prefer co-located Suspense

```tsx
// Granular section-level Suspense
import { Suspense } from "react";
import { TestimonialsSkeleton } from "@/components/skeletons/TestimonialsSkeleton";

// In page.tsx
<Suspense fallback={<TestimonialsSkeleton />}>
  <TestimonialsSection />
</Suspense>

// Dynamic import Suspense
import dynamic from "next/dynamic";
const SplineScene = dynamic(() => import("@/components/3d/SplineScene"), {
  loading: () => <HeroSceneSkeleton />,
  ssr: false,
});
```

---

## 3 · Three.js / WebGL Error Boundary

Three.js can fail silently (no WebGL support) or throw (memory, context loss). Both must be caught.

```tsx
// components/3d/WebGLErrorBoundary.tsx
"use client";
import { Component, ReactNode } from "react";
import * as Sentry from "@sentry/nextjs";

interface Props { children: ReactNode; fallback: ReactNode; }
interface State { hasError: boolean; }

export class WebGLErrorBoundary extends Component<Props, State> {
  state = { hasError: false };

  static getDerivedStateFromError() {
    return { hasError: true };
  }

  componentDidCatch(error: Error) {
    Sentry.captureException(error, { tags: { context: "webgl" } });
  }

  render() {
    return this.state.hasError ? this.props.fallback : this.props.children;
  }
}

// Usage in HeroSection.tsx
<WebGLErrorBoundary fallback={<HeroStaticFallback />}>
  <SplineScene />
</WebGLErrorBoundary>
```

WebGL support detection (run before initialising renderer):

```ts
function isWebGLSupported(): boolean {
  try {
    const canvas = document.createElement("canvas");
    return !!(
      window.WebGLRenderingContext &&
      (canvas.getContext("webgl") || canvas.getContext("experimental-webgl"))
    );
  } catch {
    return false;
  }
}
```

---

## 4 · GSAP Error Safety

Animations are enhancement, not requirement. Content must be visible if animation fails.

```ts
// Always wrap complex GSAP setups in try/catch
try {
  gsap.registerPlugin(ScrollTrigger);
  const ctx = gsap.context(() => {
    // animation setup
  }, containerRef);
  return () => ctx.revert();
} catch (error) {
  // Log to Sentry but don't crash — content remains visible
  if (process.env.NODE_ENV === "production") {
    Sentry.captureException(error, { tags: { context: "gsap" } });
  }
}
```

**Initial states:** Always use `gsap.set()` for initial states, not CSS `opacity: 0`. If GSAP fails to initialise, elements with CSS `opacity: 0` become invisible permanently.

```ts
// ✅ Safe — if GSAP fails, element is still visible
gsap.set(el, { opacity: 0, y: 30 });

// ❌ Dangerous — if GSAP fails, opacity stays 0 forever
// className="opacity-0"
```

---

## 5 · Form Error Resilience

**Rules:**
- Client-side validation before submission (react-hook-form + zod)
- Preserve all user input on error — never reset the form on failure
- Handle network errors (offline, timeout) separately from server validation errors
- Timeout: if submission takes > 10 seconds, show "still working…" message
- Never show a raw error object or stack trace to the user

```tsx
const onSubmit = async (data: FormData) => {
  setError(null);
  setIsSubmitting(true);

  // Timeout warning after 10s
  const timeoutId = setTimeout(() => setShowTimeout(true), 10_000);

  try {
    const response = await fetch("/api/contact", {
      method: "POST",
      body: JSON.stringify(data),
      headers: { "Content-Type": "application/json" },
    });

    clearTimeout(timeoutId);

    if (!response.ok) {
      const body = await response.json().catch(() => ({}));
      throw new Error(body.error ?? "Submission failed. Please try again.");
    }

    setSubmitted(true);
  } catch (err) {
    clearTimeout(timeoutId);
    // Preserve form data — do NOT call reset()
    const message = err instanceof Error ? err.message : "Something went wrong. Please try again.";
    setError(message);
    Sentry.captureException(err, { tags: { context: "contact-form" } });
  } finally {
    setIsSubmitting(false);
    setShowTimeout(false);
  }
};
```

---

## 6 · API Route Error Contract

Every API route must follow this shape:

**Success:**
```json
{ "success": true, "data": { ... } }
```

**Error:**
```json
{ "error": "Human-readable message", "code": "MACHINE_READABLE_CODE" }
```

**Standard template:**

```ts
// app/api/[route]/route.ts
import { NextResponse } from "next/server";
import { captureException } from "@sentry/nextjs";

export async function POST(request: Request) {
  try {
    const body = await request.json().catch(() => null);
    if (!body) {
      return NextResponse.json(
        { error: "Invalid request body", code: "INVALID_BODY" },
        { status: 400 }
      );
    }

    // Validate required fields
    const { name, email } = body;
    if (!name || !email) {
      return NextResponse.json(
        { error: "Name and email are required", code: "MISSING_FIELDS" },
        { status: 400 }
      );
    }

    // ... business logic ...

    return NextResponse.json({ success: true });
  } catch (error) {
    // Log full error server-side
    captureException(error, { tags: { route: "/api/contact" } });
    console.error("[API /api/contact]", error);

    // Return generic message to client — never expose internals
    return NextResponse.json(
      { error: "An unexpected error occurred. Please try again.", code: "INTERNAL_ERROR" },
      { status: 500 }
    );
  }
}
```

---

## 7 · Sentry Configuration

### Installation

```bash
npx @sentry/wizard@latest -i nextjs
```

This creates `sentry.client.config.ts`, `sentry.server.config.ts`, `sentry.edge.config.ts`, and patches `next.config.ts`.

### `sentry.client.config.ts`

```ts
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,

  // Performance monitoring
  tracesSampleRate: process.env.NODE_ENV === "production" ? 0.2 : 1.0,

  // Replay — capture 10% of sessions, 100% of error sessions
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,

  // Reduce noise
  ignoreErrors: [
    "ResizeObserver loop limit exceeded",
    "ResizeObserver loop completed with undelivered notifications",
    /^ChunkLoadError/,
    /Loading chunk \d+ failed/,
    /Failed to fetch dynamically imported module/,
    "Non-Error promise rejection captured",
  ],

  denyUrls: [
    // Browser extensions
    /extensions\//i,
    /^chrome:\/\//i,
    /^chrome-extension:\/\//i,
    /^moz-extension:\/\//i,
  ],

  integrations: [
    Sentry.replayIntegration({
      maskAllText: false,
      blockAllMedia: false,
    }),
  ],
});
```

### `sentry.server.config.ts`

```ts
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: process.env.NODE_ENV === "production" ? 0.2 : 1.0,
});
```

### `next.config.ts` Sentry wrapper

```ts
import { withSentryConfig } from "@sentry/nextjs";

const nextConfig = { /* existing config */ };

export default withSentryConfig(nextConfig, {
  org: "haide-digital",
  project: "haide-digital-production",
  silent: !process.env.CI,
  widenClientFileUpload: true,
  hideSourceMaps: true, // Don't expose source maps publicly
  disableLogger: true,
  automaticVercelMonitors: true,
});
```

### Environment Variables

```env
# .env.local
NEXT_PUBLIC_SENTRY_DSN=https://xxxxxxxx@oXXXXXX.ingest.sentry.io/XXXXXX
SENTRY_DSN=https://xxxxxxxx@oXXXXXX.ingest.sentry.io/XXXXXX
SENTRY_AUTH_TOKEN=sntrys_xxxx  # For source map upload — keep server-only
```

---

## 8 · Alert Thresholds

Configure in Sentry → Alerts:

| Alert | Condition | Notification |
|---|---|---|
| Error spike | Error rate > 5% in 5 minutes | Email + Slack |
| New error type | > 10 occurrences of unseen error | Email |
| LCP degradation | LCP > 20% above 30-day baseline | Email |
| API failures | `/api/**` 5xx rate > 2% | Email + Slack |
| Unhandled promise rejection | Any new type | Email |

---

## 9 · ChunkLoadError Recovery

Dynamic imports can fail with `ChunkLoadError` after a deployment (stale chunks). Handle gracefully:

```tsx
// hooks/useChunkErrorRecovery.ts
import { useEffect } from "react";

export function useChunkErrorRecovery() {
  useEffect(() => {
    const handleError = (event: ErrorEvent) => {
      if (
        event.message?.includes("ChunkLoadError") ||
        event.message?.includes("Loading chunk") ||
        event.message?.includes("Failed to fetch dynamically imported module")
      ) {
        event.preventDefault();
        // Auto-reload once; if it's already been reloaded, show prompt
        if (!sessionStorage.getItem("chunk_reload_attempted")) {
          sessionStorage.setItem("chunk_reload_attempted", "1");
          window.location.reload();
        } else {
          // Avoid infinite loop — show user a prompt
          // Replace with your toast/modal pattern
          if (confirm("A new version of the site is available. Reload to continue?")) {
            sessionStorage.removeItem("chunk_reload_attempted");
            window.location.reload();
          }
        }
      }
    };

    window.addEventListener("error", handleError);
    return () => window.removeEventListener("error", handleError);
  }, []);
}

// Add to layout.tsx root client wrapper component
```

---

## 10 · Logging Conventions

| Method | Use case |
|---|---|
| `console.error()` | Actual errors — unexpected states, caught exceptions |
| `console.warn()` | Expected edge cases — fallback triggered, retry attempted |
| `console.log()` | **Never in production** — dev/debug only |
| `console.info()` | **Never in production** |

Strip `console.log` and `console.info` in production via Next.js config:

```ts
// next.config.ts
const nextConfig = {
  compiler: {
    removeConsole: process.env.NODE_ENV === "production"
      ? { exclude: ["error", "warn"] }
      : false,
  },
};
```

Server-side API logs use structured JSON for Cloudflare Pages compatibility:

```ts
// Structured server log pattern
console.error(JSON.stringify({
  level: "error",
  route: "/api/contact",
  message: error.message,
  timestamp: new Date().toISOString(),
}));
```

---

## Verification Checklist

Run before every production deploy.

- [ ] `/app/error.tsx` exists, is branded, logs to Sentry
- [ ] `/app/not-found.tsx` exists, is branded, returns HTTP 404
- [ ] `/app/loading.tsx` exists with skeleton UI matching the home page layout
- [ ] All `async` Server Components are wrapped in `<Suspense>` with skeleton fallback
- [ ] All `dynamic()` imports have a `loading` fallback
- [ ] Three.js / Spline component is wrapped in `WebGLErrorBoundary` with static image fallback
- [ ] GSAP setup is in `try/catch`; initial states use `gsap.set()` not CSS opacity-0
- [ ] All API routes have `try/catch` and return `{ error, code }` shape on failure
- [ ] Contact form preserves user input on error; timeout warning at 10s
- [ ] `@sentry/nextjs` is installed and receiving test events
- [ ] Sentry `ignoreErrors` list configured (ResizeObserver, ChunkLoadError, extensions)
- [ ] `ChunkLoadError` recovery hook added to root layout
- [ ] `console.log` / `console.info` removed from production build (via `removeConsole`)
- [ ] No stack traces or internal error details exposed to the client

---

## References

- `references/sentry-setup.md` — Full Sentry wizard walkthrough, DSN configuration, source map upload CI script, alert setup guide
- `references/fallback-skeletons.md` — Per-section skeleton component patterns (Hero, Services, Testimonials, CaseStudies)
- `references/api-error-codes.md` — Complete list of error codes used across all API routes, with descriptions and HTTP status mapping
