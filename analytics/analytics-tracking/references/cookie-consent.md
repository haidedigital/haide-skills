# Cookie Consent — Haide Digital

## Table of Contents
1. Legal Requirements
2. Consent State Management
3. GTM Conditional Loading
4. Cookie Banner Component
5. Consent Categories

---

## 1. Legal Requirements

Bulgaria is an EU member state. GDPR and the ePrivacy Directive apply:

- Cookie consent required before any non-essential cookies
- Tracking scripts must not fire until explicit consent
- "Reject all" must be equally prominent as "Accept all"
- Granular choices required: Analytics / Marketing / Functional
- Pre-checked boxes are not valid consent
- Consent must be freely given, not coerced (no cookie walls)
- Users must be able to withdraw consent at any time

---

## 2. Consent State Management

```ts
// src/lib/consent.ts
type ConsentState = "pending" | "accepted" | "declined";

const CONSENT_KEY = "haide_cookie_consent";

export function getConsent(): ConsentState {
  if (typeof window === "undefined") return "pending";
  const stored = localStorage.getItem(CONSENT_KEY);
  if (stored === "accepted" || stored === "declined") return stored;
  return "pending";
}

export function setConsent(state: "accepted" | "declined") {
  localStorage.setItem(CONSENT_KEY, state);

  if (state === "accepted") {
    initializeTracking();
  }
}

export function hasConsent(): boolean {
  return getConsent() === "accepted";
}

function initializeTracking() {
  // Load GTM only after consent
  const gtmId = process.env.NEXT_PUBLIC_GTM_ID;
  if (!gtmId) return;

  // Push consent event to dataLayer
  window.dataLayer = window.dataLayer || [];
  window.dataLayer.push({
    event: "consent_granted",
    consent_type: "analytics",
  });

  // Inject GTM script
  const script = document.createElement("script");
  script.innerHTML = `
    (function(w,d,s,l,i){w[l]=w[l]||[];w[l].push({'gtm.start':
    new Date().getTime(),event:'gtm.js'});var f=d.getElementsByTagName(s)[0],
    j=d.createElement(s),dl=l!='dataLayer'?'&l='+l:'';j.async=true;j.src=
    'https://www.googletagmanager.com/gtm.js?id='+i+dl;f.parentNode.insertBefore(j,f);
    })(window,document,'script','dataLayer','${gtmId}');
  `;
  document.head.appendChild(script);
}
```

---

## 3. GTM Conditional Loading

GTM must NOT load before consent. The pattern:

1. Page loads with `window.dataLayer = []` (empty, no GTM)
2. Cookie banner appears for new visitors
3. User accepts: `setConsent("accepted")` injects GTM script
4. GTM initializes, fires GA4 `page_view`
5. Subsequent page loads: check `getConsent()`, load GTM immediately if `accepted`

**In layout.tsx:**
```tsx
"use client";

import { useEffect } from "react";
import { getConsent } from "@/lib/consent";

function ConsentGatedGTM() {
  useEffect(() => {
    // DataLayer always initializes
    window.dataLayer = window.dataLayer || [];

    // GTM only loads if consent was previously granted
    if (getConsent() === "accepted") {
      const gtmId = process.env.NEXT_PUBLIC_GTM_ID;
      if (!gtmId) return;

      const script = document.createElement("script");
      script.async = true;
      script.src = `https://www.googletagmanager.com/gtm.js?id=${gtmId}`;
      document.head.appendChild(script);

      window.dataLayer.push({ "gtm.start": new Date().getTime(), event: "gtm.js" });
    }
  }, []);

  return null;
}
```

---

## 4. Cookie Banner Component

Branded to Haide design system. Not a generic third-party widget.

```tsx
// src/components/ui/CookieBanner.tsx
"use client";

import { useEffect, useState } from "react";
import { getConsent, setConsent } from "@/lib/consent";

export default function CookieBanner() {
  const [visible, setVisible] = useState(false);

  useEffect(() => {
    if (getConsent() === "pending") {
      setVisible(true);
    }
  }, []);

  if (!visible) return null;

  function handleAccept() {
    setConsent("accepted");
    setVisible(false);
  }

  function handleDecline() {
    setConsent("declined");
    setVisible(false);
  }

  return (
    <div
      className="fixed bottom-0 inset-x-0 z-[9999] p-4 md:p-6"
      role="dialog"
      aria-label="Cookie consent"
    >
      <div className="max-w-[1560px] mx-auto bg-haide-dark rounded-2xl p-6 md:p-8 flex flex-col md:flex-row items-start md:items-center gap-4 md:gap-8 shadow-2xl">
        <div className="flex-1">
          <p className="font-poppins font-semibold text-white text-lg mb-1">
            We value your privacy
          </p>
          <p className="font-noto text-white/70 text-base leading-relaxed">
            We use cookies to analyze site performance and improve your experience.
            No tracking fires until you consent.
          </p>
        </div>
        <div className="flex gap-3 shrink-0">
          <button
            onClick={handleDecline}
            className="px-6 py-3 rounded-full border border-white/30 text-white font-poppins font-medium text-sm hover:bg-white/10 transition-colors min-h-[44px]"
          >
            Reject all
          </button>
          <button
            onClick={handleAccept}
            className="px-6 py-3 rounded-full bg-haide-orange text-white font-poppins font-semibold text-sm hover:brightness-110 transition-all min-h-[44px]"
          >
            Accept all
          </button>
        </div>
      </div>
    </div>
  );
}
```

**Rules:**
- Appears on bottom of screen, not a modal (user can still browse)
- Dark background matches Haide brand (`bg-haide-dark`)
- "Reject all" and "Accept all" are equally sized and prominent
- Min touch target 44px on both buttons
- Dismisses on either choice
- Does NOT reappear after user makes a choice

---

## 5. Consent Categories

For future granular consent (not required for MVP):

| Category | What it covers | Required for site function |
|---|---|---|
| Necessary | Session cookies, CSRF tokens | Yes (always active) |
| Analytics | GA4, scroll tracking, section views | No (requires consent) |
| Marketing | GTM marketing tags, remarketing | No (requires consent) |
| Functional | Preferences, theme, language | No (opt-in) |

**MVP:** Binary accept/decline is sufficient. Granular categories can be added post-launch.
