---
name: accessibility-a11y
description: WCAG 2.1 AA compliance standard for every Haide Digital component. Covers color contrast ratios (pre-calculated for Haide palette), keyboard navigation patterns, ARIA roles for all custom components (nav, accordions, carousels, mobile menu), focus management including skip links and focus traps, screen reader patterns for SVGs and icon-only buttons, and animation accessibility via prefers-reduced-motion. Use this skill when building any interactive component, reviewing a component before ship, or auditing an existing section. Output is a per-rule PASS / FAIL verdict with exact fix for every failure.
---

# Accessibility (A11y) — Haide Digital

## The Standard

Haide Digital is an **Organic Growth Engineering** company. An inaccessible site contradicts the brand's engineering ethos, fails ~20% of users, and sends negative quality signals to search engines. Every component must meet **WCAG 2.1 Level AA** before shipping.

**Completion rule:**
- ✅ **PASS** → ships
- ⚠️ **WARNING** → ships with a logged note; must be resolved before the next milestone
- ❌ **FAIL** → must fix before shipping. No exceptions.

---

## 1 · Color Contrast

Minimum ratios: **4.5:1** for normal text, **3:1** for large text (≥ 18px regular or ≥ 14px bold), **3:1** for UI component boundaries and graphical objects.

### Haide Palette — Pre-Calculated Ratios

| Foreground | Background | Ratio | Normal text | Large text |
|---|---|---|---|---|
| `#FFFFFF` | `#F15928` (haide-orange) | **3.03:1** | ❌ FAIL | ✅ PASS (≥ 18px) |
| `#F15928` | `#FFFFFF` | **3.03:1** | ❌ FAIL | ✅ PASS (≥ 18px) |
| `#16232A` | `#FFFFFF` | **14.4:1** | ✅ PASS | ✅ PASS |
| `#FFFFFF` | `#16232A` | **14.4:1** | ✅ PASS | ✅ PASS |
| `#233038` | `#FFFFFF` | **12.5:1** | ✅ PASS | ✅ PASS |
| `#4B5563` | `#FFFFFF` | **5.9:1** | ✅ PASS | ✅ PASS |
| `rgba(26,26,26,0.7)` | `#FFFFFF` | **~4.8:1** | ✅ PASS | ✅ PASS |
| `rgba(26,26,26,0.5)` | `#FFFFFF` | **~3.1:1** | ❌ FAIL | ✅ PASS (≥ 18px) |
| `rgba(255,255,255,0.6)` | `#16232A` | **~5.2:1** | ✅ PASS | ✅ PASS |
| `rgba(255,255,255,0.4)` | `#16232A` | **~3.1:1** | ❌ FAIL | ✅ PASS (≥ 18px) |
| `rgba(255,255,255,0.7)` | `#0A1628` | **~5.6:1** | ✅ PASS | ✅ PASS |

> ⚠️ **`#F15928` on `#FFFFFF`** (and vice versa) only passes at large text sizes. Never use haide-orange for body text on white, or white body text on an orange background. Orange is for headings (≥ 18px), CTAs (bolt text ≥ 14px bold), and decorative numbers only.

> ⚠️ **`rgba(26,26,26,0.5)` and `rgba(255,255,255,0.4)`** fail at normal text sizes. These opacity values are acceptable for decorative dividers/dots but must not be used for readable text.

### Contrast Checklist

- [ ] All body copy (< 18px regular, < 14px bold) meets 4.5:1
- [ ] All headings and large text meets 3:1 (verify per-case when orange on gradient)
- [ ] Input borders, focus rings, and UI graph boundaries meet 3:1
- [ ] No text placed directly over the hero gradient background without a verified ratio
- [ ] Muted text (e.g. `text-white/40`) never used on body copy — only decorative

---

## 2 · Focus Management

### Global Focus Ring Standard

Every interactive element must have a visible `:focus-visible` ring. The Haide on-brand style:

```css
/* globals.css */
:focus-visible {
  outline: 2px solid #F15928;
  outline-offset: 2px;
  border-radius: 4px;
}
```

- **Never** use `outline: none` or `outline: 0` without an explicit replacement
- Buttons with `rounded-full` should have `border-radius: 9999px` on the focus ring
- Focus rings must be visible on both light and dark backgrounds (the orange works on both Haide backgrounds)

### Skip-to-Content Link

Must be the **first focusable element** in the DOM, visible only on focus:

```tsx
// layout.tsx — before <Navbar />
<a
  href="#main-content"
  className="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 focus:z-[9999] focus:px-4 focus:py-2 focus:bg-haide-orange focus:text-white focus:font-poppins focus:font-semibold focus:rounded-full focus:shadow-lg"
>
  Skip to content
</a>
```

The `<main>` element must have `id="main-content"` and `tabIndex={-1}`.

### Mobile Menu Focus Trap

When the mobile menu is open:
- Focus must be trapped inside the menu (`Tab` and `Shift+Tab` cycle within)
- `Escape` must close the menu and return focus to the toggle button
- Use a `useEffect` with `keydown` listener for Escape, and a `useFocusTrap` hook or the Radix UI `Dialog` primitive

```ts
// Minimum Escape handler in Navbar.tsx
useEffect(() => {
  if (!mobileOpen) return;
  const onKey = (e: KeyboardEvent) => {
    if (e.key === "Escape") {
      setMobileOpen(false);
      toggleRef.current?.focus(); // return focus to hamburger button
    }
  };
  document.addEventListener("keydown", onKey);
  return () => document.removeEventListener("keydown", onKey);
}, [mobileOpen]);
```

### Focus Checklist

- [ ] Skip-to-content link present and functional
- [ ] Mobile menu traps focus and closes on Escape
- [ ] All modals / drawers trap focus
- [ ] After menu / modal closes, focus returns to the trigger element
- [ ] No element has `tabIndex > 0` (breaks natural tab order)
- [ ] No keyboard trap in non-modal contexts

---

## 3 · Keyboard Navigation

### Interactive Element Rules

| Element | Keyboard expectation |
|---|---|
| Links and buttons | `Tab` to focus, `Enter` or `Space` to activate |
| Accordion (FAQSection) | `Tab` to focus item, `Enter` / `Space` to expand/collapse |
| Carousel (Testimonials) | Arrow keys move slides; `Tab` reaches navigation buttons |
| Mobile menu toggle | `Enter` / `Space` opens, `Escape` closes |
| Dropdown nav (future) | `↓` opens, arrow keys move items, `Escape` closes, `Tab` exits |
| Service tabs | Arrow keys move between tabs; `Enter` / `Space` selects |

### Tab Order

- Must follow visual reading order (left-to-right, top-to-bottom)
- Never reorder visually with CSS `order` and `flex-direction: row-reverse` without adjusting the DOM order (causes a disconnect for keyboard users)
- Test by repeatedly pressing `Tab` and confirming the focus indicator tracks in the expected visual sequence

### Keyboard Checklist

- [ ] Tab through entire page — every interactive element is reachable in order
- [ ] No element is skipped or unreachable by keyboard
- [ ] Arrow-key navigation works on all custom widget patterns (tabs, accordions)
- [ ] Escape closes all modal-like overlays
- [ ] No keyboard traps outside intentional focus traps

---

## 4 · ARIA Patterns

Apply the correct ARIA roles to every custom interactive component.

### Navigation

```tsx
<nav aria-label="Main navigation" role="navigation">
  ...
</nav>
```

### Mobile Menu Button

```tsx
<button
  aria-label="Open navigation menu"
  aria-expanded={mobileOpen}
  aria-controls="mobile-menu"
  onClick={() => setMobileOpen(!mobileOpen)}
>
  {/* hamburger / close SVG */}
</button>

<div id="mobile-menu" aria-hidden={!mobileOpen} ...>
  ...
</div>
```

### FAQ Accordion

```tsx
<div role="list">
  {faqs.map((faq, i) => (
    <div key={i} role="listitem">
      <button
        aria-expanded={openIndex === i}
        aria-controls={`faq-answer-${i}`}
        id={`faq-trigger-${i}`}
      >
        {faq.question}
      </button>
      <div
        id={`faq-answer-${i}`}
        role="region"
        aria-labelledby={`faq-trigger-${i}`}
        hidden={openIndex !== i}
      >
        {faq.answer}
      </div>
    </div>
  ))}
</div>
```

### Service Tabs / Toggle

```tsx
<div role="tablist" aria-label="Services">
  {services.map((s, i) => (
    <button
      key={s.name}
      role="tab"
      aria-selected={activeTab === i}
      aria-controls={`service-panel-${i}`}
      id={`service-tab-${i}`}
    >
      {s.name}
    </button>
  ))}
</div>

{services.map((s, i) => (
  <div
    key={s.name}
    role="tabpanel"
    id={`service-panel-${i}`}
    aria-labelledby={`service-tab-${i}`}
    hidden={activeTab !== i}
  >
    {/* service content */}
  </div>
))}
```

### Stats / Metrics

Screen readers should read the full meaning, not just `+189%`:

```tsx
<span aria-label="189% increase in organic traffic">
  +189% <span aria-hidden="true">Organic Traffic</span>
</span>
```

### ARIA Checklist

- [ ] `<nav>` has `aria-label="Main navigation"`
- [ ] Mobile menu button has `aria-expanded`, `aria-controls`, `aria-label`
- [ ] Mobile menu panel has `aria-hidden` toggled
- [ ] FAQ accordion: `aria-expanded` on trigger, `aria-controls` → `role="region"`
- [ ] Service tabs: `role="tablist"` / `role="tab"` / `role="tabpanel"` / `aria-selected`
- [ ] Stats/numbers: full `aria-label` (e.g. "+142% Organic MRR increase")
- [ ] No `aria-label` duplicates the visible text (redundant but acceptable — avoid if possible)

---

## 5 · Image & SVG Accessibility

### Rules

| Scenario | Rule |
|---|---|
| Informative image (logo, product screenshot) | `alt="Descriptive text"` |
| Decorative image (background, texture, glow blob) | `alt=""` |
| Icon-only button / link | `aria-label="Action description"` on the button |
| SVG icon alongside visible text | `aria-hidden="true"` on the `<svg>` |
| SVG used alone as button icon | `<title>` inside SVG **or** `aria-label` on parent `<button>` |
| `next/image` | `alt` prop always required; never omit or use `alt="image"` |

### Common Haide Patterns

```tsx
// Logo — informative
<Image src="/logo.png" alt="Haide Digital" width={120} height={34} />

// Decorative blob / glow
<div aria-hidden="true" className="...glow blob..." />

// Icon button (carousel arrows)
<button aria-label="Next testimonial">
  <ChevronRight aria-hidden="true" className="w-6 h-6" />
</button>

// SVG inline with label beside it — hide SVG from AT
<span className="flex items-center gap-2">
  <svg aria-hidden="true" ...>{/* arrow icon */}</svg>
  View case study
</span>
```

### Image Checklist

- [ ] All `<Image>` components have meaningful `alt` (or `alt=""` if decorative)
- [ ] All icon-only buttons have `aria-label`
- [ ] Inline SVGs accompanying text have `aria-hidden="true"`
- [ ] No `alt="image"`, `alt="icon"`, or `alt` containing the filename
- [ ] Decorative blobs, gradients, and glow divs have `aria-hidden="true"`

---

## 6 · Forms

### Required Attributes

Every form input must have an associated `<label>`:

```tsx
// Explicit label (preferred)
<label htmlFor="name" className="...">Name</label>
<input id="name" name="name" type="text" required aria-required="true" ... />

// OR: aria-label when visual label is not shown
<input aria-label="Your name" ... />
```

### Error States

```tsx
<input
  aria-invalid={!!errors.email}
  aria-describedby={errors.email ? "email-error" : undefined}
  ...
/>
{errors.email && (
  <p id="email-error" role="alert" aria-live="assertive" className="text-red-500 text-sm">
    {errors.email.message}
  </p>
)}
```

### Required Field Indicators

- Do not rely on colour alone — use text "(required)" or an asterisk `*` with a legend explaining the symbol
- `aria-required="true"` on every required `<input>`, `<select>`, and `<textarea>`

### Form Checklist

- [ ] Every input has an associated `<label>` (explicit or `aria-label`)
- [ ] Required fields marked with `aria-required="true"`
- [ ] Error messages linked via `aria-describedby`
- [ ] Error messages use `role="alert"` or `aria-live="assertive"`
- [ ] Success state announced with `aria-live="polite"`
- [ ] No required indication via color alone

---

## 7 · Animation Accessibility

### `prefers-reduced-motion` — Mandatory

All animations must respect the system preference. This is a hard requirement — not advisory.

**GSAP (standard pattern):**
```ts
const mm = gsap.matchMedia();
mm.add(
  {
    noMotion: "(prefers-reduced-motion: reduce)",
    motion: "(prefers-reduced-motion: no-preference)",
  },
  (context) => {
    const { noMotion } = context.conditions as { noMotion: boolean };
    if (noMotion) return; // skip all animation setup
    // ... normal GSAP setup here
  }
);
return () => mm.revert();
```

**CSS (Tailwind + globals.css):**
```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

### Auto-Playing Content

- No animation / video / GIF autoplays for more than **5 seconds** without a pause control
- No content flashes more than **3 times per second** (seizure risk)
- Marquee / infinite scroll animations must pause on `hover` and `focus`

### Animation Checklist

- [ ] All GSAP animations gated behind `prefers-reduced-motion: no-preference`
- [ ] CSS transitions covered by `@media (prefers-reduced-motion: reduce)` in `globals.css`
- [ ] No autoplay longer than 5 seconds without a pause control
- [ ] Marquee pauses on focus/hover
- [ ] No content flashes more than 3 times/second

---

## 8 · Next.js-Specific

### Route Announcements

Next.js App Router does **not** announce route changes to screen readers by default. Add a live-region announcer:

```tsx
// components/layout/RouteAnnouncer.tsx
'use client';
import { usePathname } from 'next/navigation';
import { useEffect, useRef } from 'react';

export function RouteAnnouncer() {
  const pathname = usePathname();
  const ref = useRef<HTMLParagraphElement>(null);

  useEffect(() => {
    if (ref.current) {
      ref.current.textContent = `Navigated to ${document.title}`;
    }
  }, [pathname]);

  return (
    <p
      ref={ref}
      aria-live="assertive"
      aria-atomic="true"
      className="sr-only"
    />
  );
}
```

Add `<RouteAnnouncer />` inside `layout.tsx`.

### `next/link` and `next/image`

- `<Link href="...">` — accessible by default; add `aria-label` when the visible text is generic (e.g. "Read more" without context)
- `<Image alt="..." />` — required; never empty for informative images

### Next.js Checklist

- [ ] `<RouteAnnouncer />` present in `layout.tsx`
- [ ] `<Link>` elements with generic text have contextual `aria-label`
- [ ] All `<Image>` have `alt`

---

## Testing Protocol

Run before every component ships. Full procedure in `references/testing-protocol.md`.

### Quick Checklist

- [ ] **Keyboard-only**: Tab through entire page — every element reachable in order
- [ ] **Contrast**: Check all text/background combos against the table in Section 1
- [ ] **Screen reader** (VoiceOver / NVDA): All content reads logically; icons and decorative elements silent
- [ ] **200% zoom**: Layout does not break; no content cut off; no horizontal scroll
- [ ] **Focus rings**: Every focused element shows the haide-orange ring
- [ ] **Escape**: Closes all open overlays; focus returns to trigger
- [ ] **Skip link**: First Tab press shows the skip-to-content link
- [ ] **Reduced motion**: Animations disabled when system preference is set

---

## References

- `references/contrast-checks.md` — Full Haide palette contrast matrix with pass/fail annotations, usage guidance, and safe combinations
- `references/component-patterns.md` — Copy-paste accessible code patterns for Navbar, FAQSection, ServicesSection, TestimonialsSection, and the contact form
- `references/testing-protocol.md` — Step-by-step testing procedure: keyboard walkthrough, screen reader scripts, DevTools accessibility panel, axe-core CLI setup
