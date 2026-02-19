# Sentry Setup — Haide Digital

Full walkthrough for installing, configuring, and validating Sentry on the Haide Digital Next.js project.

---

## 1 · Installation

Run the Sentry wizard from the project root. It auto-configures Next.js App Router:

```bash
cd "D:\Work\Vibe Code\Antigravity\Haide Website - Figma Import"
npx @sentry/wizard@latest -i nextjs
```

The wizard will:
- Install `@sentry/nextjs`
- Create `sentry.client.config.ts`, `sentry.server.config.ts`, `sentry.edge.config.ts`
- Wrap `next.config.ts` with `withSentryConfig`
- Create `app/sentry-example-page/page.tsx` (delete after verification)

---

## 2 · Environment Variables

Add to `.env.local` (never commit these):

```env
NEXT_PUBLIC_SENTRY_DSN=https://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@oXXXXXX.ingest.sentry.io/XXXXXXX
SENTRY_DSN=https://xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx@oXXXXXX.ingest.sentry.io/XXXXXXX
SENTRY_AUTH_TOKEN=sntrys_xxxxxxxxxxxxxxxx
SENTRY_ORG=haide-digital
SENTRY_PROJECT=haide-digital-production
```

Add `SENTRY_AUTH_TOKEN`, `SENTRY_DSN`, `SENTRY_ORG`, `SENTRY_PROJECT` to Cloudflare Pages environment variables for production source map uploads.

---

## 3 · Final Config Files

### `sentry.client.config.ts`

```ts
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.NEXT_PUBLIC_SENTRY_DSN,
  environment: process.env.NODE_ENV,
  tracesSampleRate: process.env.NODE_ENV === "production" ? 0.2 : 1.0,
  replaysSessionSampleRate: 0.1,
  replaysOnErrorSampleRate: 1.0,

  ignoreErrors: [
    "ResizeObserver loop limit exceeded",
    "ResizeObserver loop completed with undelivered notifications",
    /^ChunkLoadError/,
    /Loading chunk \d+ failed/,
    /Failed to fetch dynamically imported module/,
    "Non-Error promise rejection captured",
    /^Script error\.?$/,
  ],

  denyUrls: [
    /extensions\//i,
    /^chrome:\/\//i,
    /^chrome-extension:\/\//i,
    /^moz-extension:\/\//i,
    /^safari-extension:\/\//i,
  ],

  integrations: [
    Sentry.replayIntegration({
      maskAllText: false,
      blockAllMedia: false,
    }),
    Sentry.browserTracingIntegration(),
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

### `sentry.edge.config.ts`

```ts
import * as Sentry from "@sentry/nextjs";

Sentry.init({
  dsn: process.env.SENTRY_DSN,
  tracesSampleRate: 0.2,
});
```

### `next.config.ts`

```ts
import { withSentryConfig } from "@sentry/nextjs";

const nextConfig = {
  // ... your existing config
  compiler: {
    removeConsole: process.env.NODE_ENV === "production"
      ? { exclude: ["error", "warn"] }
      : false,
  },
};

export default withSentryConfig(nextConfig, {
  org: process.env.SENTRY_ORG ?? "haide-digital",
  project: process.env.SENTRY_PROJECT ?? "haide-digital-production",
  silent: !process.env.CI,
  widenClientFileUpload: true,
  hideSourceMaps: true,
  disableLogger: true,
  automaticVercelMonitors: false, // We use Cloudflare Pages, not Vercel
});
```

---

## 4 · Verification

After setup, verify Sentry is receiving events:

1. Run the dev server: `npm run dev`
2. Visit `/sentry-example-page` (created by wizard) and trigger the test error
3. Check Sentry dashboard → Issues — you should see a new event within ~30 seconds
4. Delete `app/sentry-example-page/` once verified

Or trigger manually from the browser console:

```js
// In browser devtools on localhost
import("/sentry-loaded-check").catch(() => {
  throw new Error("Sentry verification test — safe to ignore");
});
```

---

## 5 · Alert Configuration

In Sentry → **Alerts** → **Create Alert Rule**:

### Alert 1: Error Rate Spike

- **Name:** Error rate spike
- **Trigger:** `errors` count > 50 in a 5-minute window
- **Actions:** Email `dev@haide.digital` + Slack `#incidents`

### Alert 2: New Error Type

- **Name:** New error type
- **Trigger:** New issue detected with ≥ 10 occurrences in 1 hour
- **Actions:** Email `dev@haide.digital`

### Alert 3: LCP Degradation (Performance Alert)

- **Name:** LCP degradation
- **Metric:** `p75(measurements.lcp)`
- **Trigger:** 20% increase over baseline in 1-hour window
- **Actions:** Email `dev@haide.digital`

### Alert 4: API 5xx Rate

- **Name:** API failures
- **Filter:** Transaction name contains `/api/`
- **Trigger:** 5xx error rate > 2% in 10-minute window
- **Actions:** Email + Slack

---

## 6 · Source Maps in CI (Cloudflare Pages)

Source maps allow readable stack traces for minified production code.

Add to Cloudflare Pages **build settings**:

```
Build command: npm run build
Environment variable: SENTRY_AUTH_TOKEN = <your token>
```

The `withSentryConfig` wrapper automatically uploads source maps during `npm run build` when `SENTRY_AUTH_TOKEN` is present. Source maps are not served publicly (`hideSourceMaps: true`).

---

## 7 · User Context (No PII)

Attach context to errors without collecting personal data:

```ts
// In a root client layout component, after page load
Sentry.setContext("session", {
  viewport: `${window.innerWidth}x${window.innerHeight}`,
  connection: (navigator as any).connection?.effectiveType ?? "unknown",
  theme: window.matchMedia("(prefers-color-scheme: dark)").matches ? "dark" : "light",
  reducedMotion: window.matchMedia("(prefers-reduced-motion: reduce)").matches,
});

// Set current page as tag (automatically done by browserTracingIntegration)
// Never set: email, name, IP, or any identifying info
```
