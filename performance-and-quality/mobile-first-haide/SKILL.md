---
name: mobile-first-haide
description: Mobile-first implementation standards for the Haide Digital website. Covers thumb zone layout, touch targets (44px minimum), breakpoint strategy, animation reduction for mid-range Android devices, Lenis smooth scroll disable pattern on touch, GSAP matchMedia, Three.js static fallbacks, prefers-reduced-motion, hamburger menu, form optimisation, fluid typography with clamp(), and the mandatory mobile audit checklist that must pass before any component is marked complete. Use when building or reviewing any component, section, page layout, animation, or interactive element on haide.digital.
---

# Mobile-First — Haide Digital

## The Standard

"Works on mobile" is not the bar. Every component must feel **engineered for mobile**, discovered on desktop.

---

## Breakpoints (Tailwind, mobile-first)

```
Default  0–639px    mobile portrait          ← design here first
sm       640px      mobile landscape / small tablet
md       768px      tablet
lg       1024px     desktop
xl       1280px     large desktop
2xl      1536px     ultrawide
```

**Tailwind pattern — always write mobile styles first, then layer up:**
```tsx
// Correct
<div className="px-5 md:px-12 lg:px-20">
<h2 className="text-3xl md:text-5xl lg:text-7xl font-poppins font-bold">

// Wrong
<div className="lg:px-20 md:px-12 px-5">  // desktop-first is harder to debug
```

Content wrapper: `max-w-[1560px] mx-auto px-5` (20px on mobile, matches brand spec).

---

## Thumb Zone Rule

Primary CTAs and interactive elements belong in the **bottom 60% of the viewport** on mobile:
- Bottom nav, primary CTA buttons: `bottom 40%` of screen
- Secondary actions (filters, info links): upper area is acceptable
- Never put the only CTA at the very top of a long section

---

## Touch Targets

| Element | Minimum size | Implementation |
|---|---|---|
| Buttons, links | 44×44 px | `min-h-[44px] min-w-[44px]` |
| Preferred | 48×48 px | `min-h-[48px] min-w-[48px]` |
| Icon-only buttons | 44×44 tappable area | use `p-3` wrapper |
| Nav links | 44px tall | `py-[10px]` on text links |

**Never rely on hover states alone.** Every hover effect needs a visible focus or active equivalent — use `:focus-visible` and `:active` in CSS. See `references/interactions.md`.

---

## Typography (Mobile)

```tsx
// Fluid headlines with clamp()
style={{ fontSize: "clamp(32px, 8vw, 92px)", lineHeight: "1.08" }}  // H1
style={{ fontSize: "clamp(26px, 5vw, 56px)", lineHeight: "1.15" }}  // H2
style={{ fontSize: "clamp(20px, 3.5vw, 36px)", lineHeight: "1.25" }} // H3

// Body
<p className="font-noto text-base leading-relaxed">  // 16px, lh 1.625 — NEVER text-sm on mobile
```

**Rules:**
- Body text minimum: `text-base` (16px) — never `text-sm` (14px) on mobile
- Line height: `leading-relaxed` (1.625) or higher for body text
- Max readable width: `max-w-[65ch]` on desktop; 100% (no max-w) on mobile

---

## Animation on Mobile

Use `gsap.matchMedia()` for all GSAP animations with different mobile/desktop behaviour. Full patterns in `references/animations.md`.

**Quick rules:**
- `window.innerWidth < 768` → reduce duration by 30–40%, remove scrub
- ScrollTrigger scrub → disable on mobile (causes jank on mid-range Android)
- Parallax → always disabled on mobile (use `translateY(0)` fallback)
- Three.js scenes → static `<img>` fallback below 768px unless scene is intentionally simple
- Framer Motion → always check `useReducedMotion()` and disable if true

```tsx
// Every animated component: add this guard
import { useReducedMotion } from "framer-motion";

const shouldReduce = useReducedMotion();
const animateVariant = shouldReduce ? {} : { opacity: 1, y: 0 };
```

---

## Lenis Smooth Scroll

Lenis automatically disables smooth scroll on touch devices. Keep this default. See `references/lenis-mobile.md` for the detection pattern and iOS momentum scroll handling.

**Never enable `smoothTouch: true`** — it fights native iOS rubber-band scroll and feels wrong.

---

## Image Rules on Mobile

```tsx
// Always lock aspect ratio — never let images distort
<Image
  src="..."
  alt="..."
  width={800}
  height={600}
  className="w-full aspect-[4/3] object-cover"  // locked ratio
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 33vw"
/>
```

**Rules:**
- Always `sizes` prop on grid images (prevents loading massive images on mobile)
- Never `object-contain` on product/hero images unless essential — use `object-cover`
- Lazy load everything below the fold; `priority` only on hero/LCP image

---

## Mobile Audit Checklist

**Run this before marking any component complete.** See `references/audit-checklist.md` for the full checklist with acceptance criteria. Short form:

- [ ] Touch targets: all interactive elements ≥ 44px
- [ ] Typography: body ≥ 16px, headings fluid with `clamp()`
- [ ] No hover-only states
- [ ] `prefers-reduced-motion` respected
- [ ] Animations tested at 375px (iPhone SE)
- [ ] No horizontal scroll introduced
- [ ] Three.js: static fallback below 768px confirmed
- [ ] Forms: correct `type` attributes, inputs ≥ 16px font
- [ ] Images: `sizes` prop, aspect-ratio locked
- [ ] Lenis not active on touch device

---

## References

- `references/animations.md` — GSAP matchMedia patterns, ScrollTrigger mobile config, Framer Motion reduced-motion, Three.js fallback
- `references/lenis-mobile.md` — Lenis detection, `syncTouch`, iOS momentum, `data-lenis-prevent`
- `references/interactions.md` — Touch-equivalent hover states, hamburger menu, form inputs, card patterns
- `references/audit-checklist.md` — Full component mobile audit with pass/fail criteria and DevTools testing steps
