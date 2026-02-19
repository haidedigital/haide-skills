---
name: analytics-tracking
description: >-
  Complete analytics and conversion tracking system for the Haide Digital website.
  Covers Google Tag Manager (GTM) integration with Next.js 14, GA4 event taxonomy,
  custom event tracking functions, cookie consent gating, scroll depth and section
  visibility tracking, form conversion events, UTM parameter handling, and Google
  Search Console setup. Use when implementing analytics, adding event tracking to
  components, configuring GTM/GA4, building cookie consent, handling UTM parameters,
  or verifying tracking is working correctly. Triggers on: analytics, GA4, GTM,
  Google Tag Manager, Google Analytics, tracking, events, conversion, dataLayer,
  cookie consent, scroll depth, UTM, Search Console, page view, form tracking,
  lead generated, CTA tracking, session recording, Hotjar, Clarity.
---

# Analytics & Tracking — Haide Digital

## Stack

| Tool | Role |
|---|---|
| Google Tag Manager (GTM) | Tag management container — loads all tracking |
| Google Analytics 4 (GA4) | Primary analytics — fires through GTM |
| Google Search Console | Organic search performance |
| Clarity or Hotjar | Session recording (optional, post-launch) |

**Rule:** GA4 fires through GTM, never via direct `next/script`. GTM is the single point of control.

---

## GTM in Next.js

Load GTM in root `layout.tsx`. Cookie consent must gate initialization.

```tsx
// src/app/layout.tsx
import Script from "next/script";

const GTM_ID = process.env.NEXT_PUBLIC_GTM_ID;

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <head>
        {/* DataLayer init before GTM */}
        <Script id="dataLayer-init" strategy="beforeInteractive">
          {`window.dataLayer = window.dataLayer || [];`}
        </Script>
      </head>
      <body>
        {/* GTM noscript fallback */}
        <noscript>
          <iframe
            src={`https://www.googletagmanager.com/ns.html?id=${GTM_ID}`}
            height="0"
            width="0"
            style={{ display: "none", visibility: "hidden" }}
          />
        </noscript>
        {children}
        {/* GTM loads after consent — see cookie consent section */}
      </body>
    </html>
  );
}
```

**Rules:**
- `window.dataLayer` must initialize before GTM script
- GTM script itself fires only after cookie consent
- Use `strategy="afterInteractive"` for the GTM script tag
- Never load GTM in `<head>` unconditionally for EU visitors

---

## Event Taxonomy

See `references/event-taxonomy.md` for the full event list with parameters.

**Conversion events (mark as conversions in GA4):**

| Event | Type | Trigger |
|---|---|---|
| `lead_generated` | Primary conversion | Contact form submitted successfully |
| `cta_click_lets_talk` | Micro-conversion | Navbar "Let's Talk" button |
| `cta_click_growth_plan` | Micro-conversion | "Get your growth plan" CTA |

**Event categories:**
- CTA events — button clicks with location context
- Form events — start, submit, error, lead generated
- Navigation events — dropdown, link click, mobile menu
- Engagement events — scroll depth, section viewed, time on page
- Outbound links — clicks to external domains

---

## Analytics Library

Create `src/lib/analytics.ts` with typed tracking functions. Components import these — never raw `dataLayer.push` in components.

```ts
// src/lib/analytics.ts

type CTAClickParams = {
  cta_text: string;
  cta_location: string;
};

type FormEventParams = {
  form_name?: string;
  error_field?: string;
  error_message?: string;
};

type SectionViewParams = {
  section_name: string;
};

function pushEvent(event: string, params?: Record<string, string | number>) {
  if (typeof window === "undefined") return;
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    event,
    page_path: window.location.pathname,
    ...params,
  });
}

export function trackCTAClick(params: CTAClickParams) {
  pushEvent("cta_click", params);
}

export function trackLetsTalk() {
  pushEvent("cta_click_lets_talk", { cta_text: "Let's Talk", cta_location: "navbar" });
}

export function trackGrowthPlan(location: string) {
  pushEvent("cta_click_growth_plan", { cta_text: "Get your growth plan", cta_location: location });
}

export function trackFormStart(formName: string) {
  pushEvent("form_start", { form_name: formName });
}

export function trackFormSubmit(formName: string) {
  pushEvent("form_submit", { form_name: formName });
  pushEvent("lead_generated", { form_name: formName });
}

export function trackFormError(params: FormEventParams) {
  pushEvent("form_error", params);
}

export function trackSectionView(params: SectionViewParams) {
  pushEvent("section_viewed", params);
}

export function trackOutboundClick(url: string, text: string) {
  pushEvent("outbound_click", { link_url: url, link_text: text });
}
```

**Usage in components:**
```tsx
import { trackCTAClick } from "@/lib/analytics";

<button onClick={() => {
  trackCTAClick({ cta_text: "Get Started", cta_location: "cta-section" });
}}>
  Get Started
</button>
```

---

## Cookie Consent

See `references/cookie-consent.md` for full implementation pattern.

**Rules:**
- No GTM/GA4 fires before consent — EU (Bulgaria) requires this
- Consent banner: first page load for new visitors
- On accept: initialize GTM, fire `page_view`
- On decline: page works without any tracking
- Store consent: `localStorage` key `haide_cookie_consent` = `accepted` | `declined`
- Banner must match Haide brand — not a generic third-party widget
- "Reject all" must be equally prominent as "Accept all"

---

## UTM Parameter Handling

Preserve UTM parameters through navigation and capture in lead events.

```ts
// src/lib/utm.ts
const UTM_KEYS = ["utm_source", "utm_medium", "utm_campaign", "utm_term", "utm_content"];

export function captureUTMs(): Record<string, string> {
  if (typeof window === "undefined") return {};
  const params = new URLSearchParams(window.location.search);
  const utms: Record<string, string> = {};
  for (const key of UTM_KEYS) {
    const val = params.get(key);
    if (val) utms[key] = val;
  }
  if (Object.keys(utms).length > 0) {
    sessionStorage.setItem("haide_utms", JSON.stringify(utms));
  }
  return utms;
}

export function getStoredUTMs(): Record<string, string> {
  if (typeof window === "undefined") return {};
  const stored = sessionStorage.getItem("haide_utms");
  return stored ? JSON.parse(stored) : {};
}
```

Include stored UTMs in the `lead_generated` event automatically.

---

## Search Console

- Verify via DNS TXT record (Cloudflare DNS panel)
- Do NOT use meta tag verification (fragile, breaks on redeploy)
- Connect GA4 property to Search Console for combined reporting
- Monitor: Core Web Vitals, Coverage errors, Search Performance

---

## Verification Checklist

- [ ] GTM fires on all pages (verify with GTM Preview mode)
- [ ] GA4 receiving real-time data (GA4 > Realtime report)
- [ ] All conversion events firing correctly (GA4 > DebugView)
- [ ] Scroll depth tracking active
- [ ] Cookie consent gates all tracking (decline, verify no network requests)
- [ ] No duplicate `page_view` events (check GA4 DebugView)
- [ ] UTM parameters captured in lead events
- [ ] Search Console verified and connected to GA4

---

## References

- `references/event-taxonomy.md` — Full event list with parameters, dataLayer push examples, IntersectionObserver patterns for section tracking, scroll depth implementation
- `references/cookie-consent.md` — Cookie consent component, GTM conditional loading, consent state management, Haide-branded banner pattern
