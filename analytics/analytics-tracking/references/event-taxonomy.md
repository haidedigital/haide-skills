# Event Taxonomy — Haide Digital

## Table of Contents
1. CTA Events
2. Form Events
3. Navigation Events
4. Engagement Events
5. Outbound Links
6. Scroll Depth Implementation
7. Section Visibility Tracking
8. Time on Page Milestones

---

## 1. CTA Events

| Event Name | Trigger | Parameters |
|---|---|---|
| `cta_click` | Any primary CTA button | `cta_text`, `cta_location`, `page_path` |
| `cta_click_lets_talk` | Navbar "Let's Talk" button | `cta_text`, `cta_location` |
| `cta_click_growth_plan` | "Get your growth plan" CTA | `cta_text`, `cta_location` |

**DataLayer push:**
```ts
window.dataLayer.push({
  event: "cta_click",
  cta_text: "Get your growth plan",
  cta_location: "hero",
  page_path: window.location.pathname,
});
```

**`cta_location` values:**
- `hero` — HeroSection primary CTA
- `navbar` — Navbar "Let's Talk" button
- `cta-section` — CTASection at page bottom
- `services` — ServicesSection CTA
- `mobile-menu` — Mobile nav CTA
- `footer` — Footer CTA link

---

## 2. Form Events

| Event Name | Trigger | Parameters |
|---|---|---|
| `form_start` | User focuses first form field | `form_name` |
| `form_submit` | Successful form submission | `form_name` |
| `form_error` | Validation error shown | `form_name`, `error_field`, `error_message` |
| `lead_generated` | Form submitted successfully | `form_name`, UTM params |

**`lead_generated` is the primary conversion event.** Mark this as a conversion in GA4.

```ts
// On successful submission
window.dataLayer.push({
  event: "lead_generated",
  form_name: "contact",
  page_path: window.location.pathname,
  // Include stored UTMs
  ...getStoredUTMs(),
});
```

**Form start tracking (fire once per session):**
```tsx
const [formStarted, setFormStarted] = useState(false);

function handleFirstFocus() {
  if (!formStarted) {
    trackFormStart("contact");
    setFormStarted(true);
  }
}

<Input onFocus={handleFirstFocus} />
```

---

## 3. Navigation Events

| Event Name | Trigger | Parameters |
|---|---|---|
| `nav_dropdown_open` | Dropdown menu opened | `dropdown_name` |
| `nav_link_click` | Nav item clicked | `link_text`, `link_href` |
| `mobile_menu_open` | Hamburger menu opened | — |
| `mobile_menu_close` | Mobile menu closed | — |

```ts
window.dataLayer.push({
  event: "nav_link_click",
  link_text: "Services",
  link_href: "/services",
});
```

---

## 4. Engagement Events

| Event Name | Trigger | Parameters |
|---|---|---|
| `scroll_depth` | 25%, 50%, 75%, 90% milestones | `depth_percentage` |
| `section_viewed` | Section enters viewport | `section_name` |
| `time_on_page` | 30s, 60s, 120s, 300s milestones | `time_seconds` |
| `video_play` | Video starts playing | `video_title` |
| `video_complete` | Video finishes | `video_title` |

---

## 5. Outbound Links

| Event Name | Trigger | Parameters |
|---|---|---|
| `outbound_click` | Click on external domain link | `link_url`, `link_text` |

**Auto-detect outbound links:**
```ts
// src/lib/analytics.ts
export function initOutboundTracking() {
  document.addEventListener("click", (e) => {
    const link = (e.target as HTMLElement).closest("a");
    if (!link) return;

    const href = link.getAttribute("href");
    if (!href) return;

    try {
      const url = new URL(href, window.location.origin);
      if (url.hostname !== window.location.hostname) {
        trackOutboundClick(href, link.textContent?.trim() || "");
      }
    } catch {
      // Invalid URL, skip
    }
  });
}
```

---

## 6. Scroll Depth Implementation

Fire events at 25%, 50%, 75%, and 90% scroll depth. Fire each milestone only once per page.

```ts
// src/lib/scroll-tracking.ts
const milestones = [25, 50, 75, 90];
const fired = new Set<number>();

export function initScrollTracking() {
  if (typeof window === "undefined") return;

  function checkScroll() {
    const scrollTop = window.scrollY;
    const docHeight = document.documentElement.scrollHeight - window.innerHeight;
    if (docHeight <= 0) return;

    const percent = Math.round((scrollTop / docHeight) * 100);

    for (const milestone of milestones) {
      if (percent >= milestone && !fired.has(milestone)) {
        fired.add(milestone);
        window.dataLayer = window.dataLayer || [];
        window.dataLayer.push({
          event: "scroll_depth",
          depth_percentage: milestone,
          page_path: window.location.pathname,
        });
      }
    }
  }

  window.addEventListener("scroll", checkScroll, { passive: true });
  return () => window.removeEventListener("scroll", checkScroll);
}
```

---

## 7. Section Visibility Tracking

Use IntersectionObserver to track when key sections enter the viewport.

```ts
// src/hooks/use-section-tracking.ts
"use client";

import { useEffect, useRef } from "react";

const trackedSections = new Set<string>();

export function useSectionTracking(sectionName: string) {
  const ref = useRef<HTMLElement>(null);

  useEffect(() => {
    const el = ref.current;
    if (!el || trackedSections.has(sectionName)) return;

    const observer = new IntersectionObserver(
      ([entry]) => {
        if (entry.isIntersecting && !trackedSections.has(sectionName)) {
          trackedSections.add(sectionName);
          window.dataLayer = window.dataLayer || [];
          window.dataLayer.push({
            event: "section_viewed",
            section_name: sectionName,
            page_path: window.location.pathname,
          });
          observer.disconnect();
        }
      },
      { threshold: 0.3 }
    );

    observer.observe(el);
    return () => observer.disconnect();
  }, [sectionName]);

  return ref;
}
```

**Usage in sections:**
```tsx
import { useSectionTracking } from "@/hooks/use-section-tracking";

export default function ServicesSection() {
  const ref = useSectionTracking("services");
  return <section ref={ref}>{/* ... */}</section>;
}
```

**Tracked sections:**
- `hero`
- `problem`
- `services`
- `process`
- `industries`
- `comparison`
- `testimonials`
- `case-studies`
- `faq`
- `cta`

---

## 8. Time on Page Milestones

Track engaged time milestones at 30s, 60s, 120s, and 300s.

```ts
// src/lib/time-tracking.ts
const timeMilestones = [30, 60, 120, 300];
const firedTimes = new Set<number>();

export function initTimeTracking() {
  if (typeof window === "undefined") return;

  const start = Date.now();

  const interval = setInterval(() => {
    const elapsed = Math.floor((Date.now() - start) / 1000);

    for (const milestone of timeMilestones) {
      if (elapsed >= milestone && !firedTimes.has(milestone)) {
        firedTimes.add(milestone);
        window.dataLayer = window.dataLayer || [];
        window.dataLayer.push({
          event: "time_on_page",
          time_seconds: milestone,
          page_path: window.location.pathname,
        });
      }
    }

    // Clear interval after all milestones fired
    if (firedTimes.size === timeMilestones.length) {
      clearInterval(interval);
    }
  }, 5000);

  return () => clearInterval(interval);
}
```
