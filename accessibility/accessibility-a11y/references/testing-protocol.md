# A11y Testing Protocol — Haide Digital

Step-by-step procedure for accessibility testing every component before ship.

---

## 1 · Keyboard-Only Walkthrough

**Goal:** Every interactive element is reachable and operable without a mouse.

1. Close all other browser tabs, focus the page
2. Press `Tab` repeatedly and record the focus order
3. Verify:
   - [ ] Skip-to-content link appears on first `Tab` press
   - [ ] Focus moves left→right, top→bottom through the page
   - [ ] Navbar links and CTA button are reachable
   - [ ] Mobile menu (at narrow viewport): toggle button reachable, menu opens, links reachable, `Escape` closes it
   - [ ] FAQ accordion: all question buttons reachable; `Enter`/`Space` expands and collapses
   - [ ] Testimonials carousel: prev/next buttons reachable by Tab
   - [ ] Contact form: all inputs, dropdowns, and submit button reachable
   - [ ] Footer links reachable
4. At no point is focus lost, hidden, or trapped unexpectedly

**Tools:** Chrome DevTools → Sources → "Pause on caught exceptions" to catch focus issues; or simply tab through manually.

---

## 2 · Contrast Verification

**Goal:** All text meets WCAG AA ratios. Reference `contrast-checks.md` first.

1. Open [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) or use Chrome DevTools → Elements → computed color
2. Spot-check each section:

| Section | Key combinations to verify |
|---|---|
| Hero | White text on dark gradient |
| Services (dark bg) | White/orange text on #16232A |
| Industries (white bg) | Dark text on white |
| Comparison (dark bg) | White text on dark |
| Testimonials (dark bg) | White/muted text |
| Case Studies (white bg) | Dark text, orange accents |
| FAQ (white bg) | Dark text, orange numbers |
| CTA (white bg) | Dark and gray text |
| Contact form | Labels and input text on dark bg |

3. Flag any combination scoring < 4.5:1 for normal text or < 3:1 for large text

---

## 3 · Screen Reader Test

**Windows:** NVDA (free) + Chrome  
**Mac:** VoiceOver (built-in) — `Cmd+F5` to toggle

### VoiceOver Script

1. Enable VoiceOver, navigate to page
2. Press `VO+U` to open the Rotor — check:
   - **Headings list**: H1 → H2 → H3 hierarchy is logical
   - **Links list**: No generic "click here" or "read more" links without context
   - **Form controls**: Every input reads its label when focused
3. Tab through interactive elements — confirm:
   - Buttons read their label (not just "button")
   - Icon-only buttons read their `aria-label`
   - Decorative SVGs are silent (no announcement)
   - FAQ accordion reads "expanded" / "collapsed"
   - Stats read the full `aria-label` (e.g. "+142% increase in organic MRR")
4. Navigate to `/contact`, fill the form — confirm:
   - Labels are announced when input is focused
   - Error messages are announced when validation fires
   - Success message is announced after submission

### NVDA Script (Windows)

1. Open NVDA, browse to page
2. Press `H` to jump between headings — verify hierarchy
3. Press `F` to jump between form fields — verify labels are read
4. Press `B` to jump between buttons — verify all have meaningful text

---

## 4 · 200% Zoom Test

1. Set browser zoom to 200% (`Ctrl+` / `Cmd+`)
2. Verify:
   - [ ] No text is cut off or overlapping
   - [ ] No horizontal scrollbar appears (check at 320px equivalent width)
   - [ ] Cards and grids reflow gracefully
   - [ ] Navbar wraps or collapses to mobile menu appropriately
   - [ ] All text remains readable — no truncation without `...` accessible alternative

---

## 5 · Focus Ring Audit

1. Tab through every interactive element
2. Verify the haide-orange 2px focus ring is visible on every element:
   - Nav links
   - CTA buttons (pill shape — ring follows `border-radius: 9999px`)
   - FAQ accordion buttons
   - Carousel buttons
   - Form inputs and submit
   - Footer links
3. Check on both light and dark backgrounds — orange ring must be visible on both

**Red flags:**
- Focus jumps with no visible indicator → `outline: none` without replacement
- Focus ring appears but is hidden behind another element → check `z-index` and `overflow: hidden`

---

## 6 · Automated Scan with axe DevTools

Install the [axe DevTools browser extension](https://www.deque.com/axe/devtools/) or use the CLI:

```bash
npx axe-core https://localhost:3000 --reporter=v2
```

Or integrate in CI:

```bash
npm install --save-dev @axe-core/cli
npx axe http://localhost:3000 --exit
```

**Expected:** 0 violations at WCAG 2.1 AA level.

Common issues axe detects:
- Missing `alt` on images
- Missing `aria-label` on icon buttons
- Insufficient contrast
- Missing form labels
- Duplicate IDs

---

## 7 · Reduced Motion Test

1. Enable "Reduce Motion" in OS settings:
   - **macOS:** System Preferences → Accessibility → Display → Reduce Motion
   - **Windows:** Settings → Ease of Access → Display → Show animations → Off
2. Reload the page
3. Verify:
   - [ ] Hero GSAP animations do not play
   - [ ] Industries scroll-stack animation does not play (cards shown statically stacked or flat)
   - [ ] Navbar entrance animation does not play
   - [ ] Testimonials carousel transitions are instant, not animated
   - [ ] FAQ accordion opens/closes without transition

---

## Pass Criteria Summary

A component passes the a11y audit when:

| Test | Required |
|---|---|
| Keyboard only — all elements reachable | ✅ Required |
| All contrast ratios pass WCAG AA | ✅ Required |
| Screen reader reads all content logically | ✅ Required |
| 200% zoom — no content lost | ✅ Required |
| Focus rings visible on all elements | ✅ Required |
| axe-core: 0 violations | ✅ Required |
| Reduced motion respected | ✅ Required |
| `Escape` closes all overlays | ✅ Required |
| Skip-to-content works | ✅ Required |
