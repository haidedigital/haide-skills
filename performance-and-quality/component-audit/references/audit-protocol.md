# Audit Protocol — Detailed Rules

This document provides the full question set, red flag triggers, and fix guidance for each section of the component audit. The `SKILL.md` tables are the quick reference; this is the source of truth.

---

## Section 1: Brand Audit

**Source skill:** `haide-design-system/SKILL.md`

### Colour

**Questions:**
- Are all colours defined as Tailwind tokens (`text-haide-orange`, `bg-haide-dark`, etc.) rather than hard-coded hex values?
- Is the primary orange (`#F15928`) used only for CTAs, accents, and active states — never as a background fill for large areas?
- Is `#16232A` (dark) the dominant background colour?
- Does the component avoid garish colour combinations not present in the design system?

**Red flags:**
```tsx
// ❌ Hard-coded hex
style={{ color: "#F15928" }}
className="text-[#F15928]"

// ❌ Off-brand colour
className="bg-blue-500 text-green-300"
```

**Fix:** Replace with Tailwind tokens (`text-haide-orange`, `bg-haide-dark/80`, etc.). Add missing tokens to `tailwind.config.ts` if needed.

### Typography

**Questions:**
- Are all headings using `font-poppins font-bold` (or `font-semibold` for subheadings)?
- Is body text using `font-noto text-base leading-relaxed` or larger?
- Is no heading using Noto Sans?
- Is no body text using Poppins?

**Red flags:**
```tsx
<p className="font-poppins text-sm"> // Poppins on body + too small
<h2 className="font-noto">           // Body font on heading
```

**Fix:** Apply correct font class per content role. Never mix heading/body font families.

### Tone & Copy (brief)

**Questions:**
- Scan all string literals for forbidden words: `innovative`, `leverage`, `cutting-edge`, `disruptive`, `synergy`, `solutions`, `seamlessly`, `transformative`, `utilize`
- Does the voice feel confident and direct, not corporate and passive?

**Fix:** See Section 6 (Copy Audit) for full replacement guidance.

---

## Section 2: Architecture Audit

**Source skill:** `haide-nextjs-architecture/SKILL.md`

### Server vs Client boundary

**Questions:**
- Does the component contain `useState`, `useEffect`, `useRef`, `useCallback`, `useMemo`, `useContext`? → Must be `"use client"`
- Does the component have event handlers (`onClick`, `onChange`, `onSubmit`)? → Must be `"use client"`
- Does the component access `window`, `document`, or `localStorage`? → Must be `"use client"` or access inside `useEffect`
- If it is a Server Component: does it fetch its own data? If so, does it use `async/await` at the top level?
- Is `"use client"` placed at the top of the file, before imports?

**Red flag: unnecessary client component**
```tsx
// ❌ Makes entire tree client just for a className state
"use client";
export default function ServicesSection() {
  return <div className="grid">...</div>; // no hooks, no events
}

// ✅ No directive needed — Server Component
export default function ServicesSection() {
  return <div className="grid">...</div>;
}
```

### TypeScript

**Questions:**
- Are all component props typed with an explicit interface or type?
- Is `any` used anywhere? → Immediate FAIL
- Are async function return types annotated?
- Are all third-party library objects typed (THREE.Mesh, gsap.core.Tween, etc.)?

**Red flags:**
```ts
// ❌
function Card({ data }: any) {}
const ref = useRef<any>(null);
```

### File conventions

**Checklist:**
- Component file: `PascalCase.tsx`
- Utility / helper: `camelCase.ts`
- Located in correct directory:
  - `src/components/sections/` — full-page sections
  - `src/components/ui/` — reusable UI atoms / molecules
  - `src/components/layout/` — Navbar, Footer, wrappers
  - `src/3d/` — Three.js / R3F components
  - `src/lib/` — utilities, data fetching, constants

### Imports

**Questions:**
- Is GSAP imported as `import gsap from "gsap"` (core only, ≤ 30KB)? ✅
- Are GSAP plugins imported inside `useEffect` or `next/dynamic`? ✅
- Is Three.js wrapped in `next/dynamic` with `{ ssr: false }`? ✅
- Any import from a package where only one function is needed? → Check for tree-shaking.

---

## Section 3: Animation Audit

**Source skill:** `gsap-haide-animations/SKILL.md`

### Motion language

**Questions:**
- Are entrance animations using the approved Haide easing curves?
  - Standard in: `cubic-bezier(0.16, 1, 0.3, 1)` (power4 out)
  - Standard out: `power2.in`
  - Smooth scroll: `power4.inOut`
- Do animations follow the Haide hierarchy — hero elements first, then supporting content staggered?
- Are durations within range? Mobile: 0.3–0.5s. Desktop: 0.5–0.9s. Hero reveals: up to 1.2s.

### Cleanup

**Questions:**
- Does every `useEffect` that creates a GSAP animation return a cleanup function?
- Is `mm.revert()` called for `gsap.matchMedia()` on unmount?
- Are all `ScrollTrigger` instances created within a context that is killed on unmount?

**Red flag:**
```tsx
// ❌ No cleanup — ScrollTrigger leaks on route change
useEffect(() => {
  gsap.to(ref.current, { opacity: 1, scrollTrigger: { ... } });
}, []);

// ✅
useEffect(() => {
  const ctx = gsap.context(() => {
    gsap.to(ref.current, { opacity: 1, scrollTrigger: { ... } });
  }, ref);
  return () => ctx.revert();
}, []);
```

### Reduced motion

**Questions:**
- Is `useReducedMotion()` (Framer Motion) or `gsap.matchMedia()` with `(prefers-reduced-motion: reduce)` used?
- When reduced motion is active, are elements immediately visible (not hidden)?

### Compositor safety

- Are only `x`, `y`, `scale`, `rotation`, `opacity` animated? → ✅
- Any animation of `width`, `height`, `top`, `left`, `padding`, `margin`, `backgroundColor` affecting large areas? → ❌ FAIL

---

## Section 4: Mobile Audit

**Source skill:** `mobile-first-haide/SKILL.md`

### Viewport

**Inspection:**
- Open DevTools → 375px width
- Scroll through entire component
- Check for horizontal scrollbar — must not appear
- Check for text clipping, overflow hidden cutting content

### Touch targets

**Inspection method:**
```
DevTools → Elements → Inspect each <button>, <a>, <input>
Computed → check height — must be ≥ 44px
```
Or: enable DevTools accessibility overlay to highlight interactive elements.

### Hover parity

**Questions:**
- Does every `:hover` have an equivalent `:active` state for touch? (Check CSS or Tailwind classes)
- Is any information or state change only revealed on hover? → FLAG (add `:active` equivalent or make visible by default)

### Animation on mobile

**Questions:**
- Is `gsap.matchMedia()` used to differentiate mobile and desktop animations?
- Is `scrub` disabled on mobile (`max-width: 767px`)?
- Is parallax returning `y: 0` on mobile?
- Are animation durations ≤ 0.5s on mobile?

---

## Section 5: Performance Audit

**Source skill:** `performance-budget/SKILL.md`

### Images

**Code checks:**
```tsx
// Every <Image> must have all three:
<Image
  width={800}    // explicit — prevents CLS
  height={600}   // explicit — prevents CLS
  sizes="..."    // correct — prevents oversized download
/>
```

**Grep checks:**
```bash
# Find any raw <img> tags (should be zero)
grep -rn "<img " src/components/

# Find Image without sizes
grep -B2 -A5 "<Image" src/components/ | grep -L "sizes"
```

### Bundle

**Questions:**
- What is the estimated gzipped size of all new code added?
- Is any new third-party library being imported? → Check its size on bundlephobia.com
- Is any component > 30KB imported directly (not dynamically)?

### Layout thrash

**Inspect via DevTools Performance tab:**
1. Record 5 seconds of interaction/scroll
2. Look for "Recalculate Style" + "Layout" events firing consecutively in rapid succession during animation
3. Each pair is a layout thrash → FAIL

### CLS

**Questions:**
- Does any element change size or position during load without pre-reserved space?
- Do images have aspect-ratio containers?
- Do fonts use `next/font` (no layout shift from font swap)?

---

## Section 6: Copy Audit

**Source skill:** `haide-copy-voice/SKILL.md` | **Reference:** `Editorial_Copywriting.md`

### Forbidden word scan

Scan all string literals and JSX text content for:

| Forbidden | Replace with |
|---|---|
| innovative / innovation | specific result or method |
| leverage | use, apply, deploy |
| cutting-edge | specific technology name |
| seamlessly | remove or describe what's smooth |
| solutions | specific deliverable |
| disruptive | specific change or outcome |
| transformative | specific impact |
| synergy | specific collaboration benefit |
| utilize | use |
| empower | help, give, build |
| journey | project, process, work |
| next-level | specific metric or outcome |

### CTA quality

**Questions:**
- Is every CTA a specific action + outcome? ("Book a discovery call" ✅ | "Get in touch" ❌)
- Is the primary CTA the most visually dominant element in the section?
- Is the CTA copy consistent with the Haide brand voice throughout the component?

### Placeholders

**Grep:**
```bash
grep -rni "lorem ipsum\|placeholder\|TODO\|FIXME\|coming soon\|TBD" src/
```
→ Any result = FAIL

---

## Section 7: SEO Audit

**Source skill:** `haide-seo/SKILL.md`

*(Skip headings 7.3–7.7 for UI-only components like buttons, cards, modals. Full audit required for page-level components and full sections.)*

### Meta tags (page-level components only)

**Check `src/app/[page]/page.tsx`:**
```tsx
export const metadata: Metadata = {
  title: "...", // unique per page, < 60 chars
  description: "...", // unique, 120–160 chars, includes keyword
  openGraph: { ... },
  twitter: { ... },
};
```

### Heading hierarchy

**Questions:**
- Is there exactly one `<h1>` on the page?
- Does this component introduce any heading? If so, is it `<h2>` or lower (not `<h1>` in a sub-section)?
- Does the heading order flow logically (no H3 before H2)?

**grep:**
```bash
grep -n "<h1\|<h2\|<h3" src/components/sections/ComponentName.tsx
```

### Images alt text

**grep:**
```bash
grep -n "alt=" src/components/sections/ComponentName.tsx
```
Every `<Image>` must have a descriptive `alt`. Red flags:
- `alt=""` on a content image (acceptable on decorative only)
- `alt="image"`, `alt="photo"`, `alt="img"` → FAIL

### Structured data

**Check if applicable:**
- Organisation: on homepage, about, contact
- Service: on services page/section
- BreadcrumbList: on inner pages
- FAQPage: on any FAQ section

If structured data exists, validate at: `https://validator.schema.org/`

---

## Section 8: Security Audit

**Source skill:** `security-headers/SKILL.md`

*(Required for components with forms, API routes, user input, or third-party scripts. Mark N/A for pure display components.)*

### Input handling

**Questions:**
- Does the component accept user input (form fields, search, URL params)?
- Is all input validated server-side with a Zod schema (not just frontend)?
- Is input sanitized before storage or rendering?
- Is `dangerouslySetInnerHTML` used? If so, is the content sanitized with DOMPurify or equivalent?

**Red flags:**
```tsx
// ❌ Raw user input rendered
<div dangerouslySetInnerHTML={{ __html: userMessage }} />

// ❌ No server-side validation
export async function POST(request: Request) {
  const { name, email } = await request.json();
  // no validation, directly used
}
```

### Form security

**Questions:**
- Does the form have a honeypot field (hidden input that bots fill)?
- Does the API route validate the request origin (CSRF protection)?
- Is there rate limiting on the API endpoint?
- Is PII (name, email, phone) logged to `console.log`? → FAIL

**Red flag:**
```ts
// ❌ PII in logs
console.log("Form submission:", { name, email, message });
```

### Third-party scripts

**Questions:**
- Are any new `<script>` tags or `next/script` imports added?
- Are tracking scripts gated behind cookie consent?
- Do script sources match the Content-Security-Policy allowlist?

---

## Section 9: Accessibility Audit

**Source skill:** `accessibility-a11y/SKILL.md`

### Contrast

**Questions:**
- Does all text meet WCAG AA ratios? (4.5:1 normal text, 3:1 large text ≥ 18px)
- Check the Haide palette table in `accessibility-a11y/SKILL.md` Section 1
- Is haide-orange (`#F15928`) used only on large text (≥ 18px) or bold text (≥ 14px bold)?
- Are any `text-white/40` or similar low-opacity values used on readable text? → FAIL

### Focus & keyboard

**Questions:**
- Does every interactive element show a `:focus-visible` ring?
- Can every interactive element be reached via `Tab` in visual reading order?
- Do custom widgets (accordion, tabs, carousel) support expected keyboard patterns?
  - Accordion: `Enter`/`Space` to expand/collapse
  - Tabs: arrow keys to move between tabs
  - Carousel: arrow keys or prev/next buttons reachable by Tab
- Does `Escape` close overlays and return focus to the trigger?

**Red flags:**
```css
/* ❌ Focus ring removed with no replacement */
outline: none;
*:focus { outline: 0; }
```

### ARIA

**Questions:**
- Does `<nav>` have `aria-label="Main navigation"`?
- Do toggle buttons have `aria-expanded` and `aria-controls`?
- Do icon-only buttons have `aria-label`?
- Do decorative SVGs and blobs have `aria-hidden="true"`?
- Do all `<Image>` components have meaningful `alt` (or `alt=""` if decorative)?

**Red flags:**
```tsx
// ❌ Icon button without label
<button onClick={toggle}><ChevronRight /></button>

// ✅ Accessible
<button aria-label="Next slide" onClick={toggle}>
  <ChevronRight aria-hidden="true" />
</button>
```

### Reduced motion

**Questions:**
- Are all animations gated behind `prefers-reduced-motion: no-preference`?
- When reduced motion is active, are elements immediately visible?
- Does the `@media (prefers-reduced-motion: reduce)` rule exist in `globals.css`?

---

## Section 10: Error Handling Audit

**Source skill:** `error-handling-monitoring/SKILL.md`

*(Required for pages, async components, dynamic imports, 3D scenes, and forms. Mark N/A for static display-only components.)*

### Suspense & error boundaries

**Questions:**
- Is every `async` Server Component wrapped in `<Suspense>` with a skeleton fallback?
- Is every `dynamic()` import wrapped in `<Suspense>` or using the `loading` option?
- Is the Three.js / WebGL scene wrapped in a `WebGLErrorBoundary` with a static image fallback?

**Red flags:**
```tsx
// ❌ Dynamic import with no fallback
const Heavy = dynamic(() => import("@/components/Heavy"));

// ✅ With fallback
const Heavy = dynamic(() => import("@/components/Heavy"), {
  loading: () => <HeavySkeleton />,
  ssr: false,
});
```

### Animation safety

**Questions:**
- Is GSAP setup wrapped in `try/catch`?
- Do initial animation states use `gsap.set()` instead of CSS `opacity: 0`?
- If GSAP fails, is content still visible?

**Red flag:**
```tsx
// ❌ If GSAP fails to load, element stays invisible forever
<div className="opacity-0">Content</div>

// ✅ Safe — gsap.set runs after JS loads; if it fails, element is visible
// (no opacity-0 in markup)
```

### Form resilience

**Questions:**
- Does the form preserve all user input on submission error?
- Is there a timeout warning if submission takes > 10 seconds?
- Are error messages user-friendly — no raw error objects or stack traces?
- Does the success/error state use branded Haide UI?

### API error contract

**Questions:**
- Does the API route return `{ error: string, code: string }` on failure?
- Is the error caught in a `try/catch` with Sentry logging?
- Is the client-facing error message generic (no internal details)?
