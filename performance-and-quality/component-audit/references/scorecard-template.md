# Component Audit Scorecard

Copy this template for each component audit. Fill in the verdict (✅ PASS / ⚠️ WARNING / ❌ FAIL) and list all issues found with file:line references and exact fixes.

---

```
COMPONENT AUDIT SCORECARD
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Component:  [ComponentName]
File:       src/components/[path]/ComponentName.tsx
PR / branch: [feature/branch-name]
Auditor:    [AI / Developer name]
Date:       [YYYY-MM-DD]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. BRAND AUDIT                                           [ ]
   ref: haide-design-system/SKILL.md
   ─────────────────────────────────
   Colors:      [ ] no hard-coded hex  [ ] correct token usage
   Typography:  [ ] Poppins headings   [ ] Noto body  [ ] 16px+ body
   Copy:        [ ] no forbidden words [ ] on-brand tone
   Feel:        [ ] looks like Haide Digital

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

2. ARCHITECTURE AUDIT                                    [ ]
   ref: haide-nextjs-architecture/SKILL.md
   ─────────────────────────────────
   RSC/Client:  [ ] correct boundary   [ ] "use client" at leaf
   TypeScript:  [ ] no `any`           [ ] all props typed
   Conventions: [ ] correct file name  [ ] correct directory
   Imports:     [ ] heavy imports dynamic  [ ] no barrel imports

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

3. ANIMATION AUDIT                                       [ ]
   ref: gsap-haide-animations/SKILL.md
   ─────────────────────────────────
   Motion lang: [ ] correct easing     [ ] duration in range
   Cleanup:     [ ] ctx.revert()       [ ] ScrollTrigger killed
   A11y:        [ ] reduced-motion handled
   Compositor:  [ ] transform/opacity only  [ ] will-change lifecycle

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

4. MOBILE AUDIT                                          [ ]
   ref: mobile-first-haide/SKILL.md
   ─────────────────────────────────
   Layout:      [ ] 375px no overflow  [ ] no horizontal scroll
   Targets:     [ ] all interactive ≥ 44px
   States:      [ ] :active equivalents for all :hover
   Typography:  [ ] body ≥ 16px at all breakpoints
   Animation:   [ ] simplified on mobile  [ ] no scrub on mobile
   Three.js:    [ ] static fallback < 768px (if applicable)

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

5. PERFORMANCE AUDIT                                     [ ]
   ref: performance-budget/SKILL.md
   ─────────────────────────────────
   Images:      [ ] next/image only    [ ] width+height set
               [ ] sizes prop correct  [ ] priority on LCP only
   Bundle:      [ ] no new initial JS > 30KB
   Animations:  [ ] compositor-safe    [ ] no layout thrash
   CLS:         [ ] aspect ratios set  [ ] no dimension shifts
   Resources:   [ ] no render-blocking additions

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

6. COPY AUDIT                                            [ ]
   ref: haide-copy-voice/SKILL.md
   ─────────────────────────────────
   Forbidden:   [ ] no banned words scanned
   CTAs:        [ ] specific + action-oriented
   Voice:       [ ] no passive voice in key actions
   Placeholders:[ ] no lorem ipsum / TODO text
   Tone:        [ ] matches section purpose

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

7. SEO AUDIT                                             [ ]
   ref: haide-seo/SKILL.md
   (page-level components only — mark N/A if UI atom)
   ─────────────────────────────────
   Meta:        [ ] unique title  [ ] unique description
   Headings:    [ ] one H1  [ ] correct hierarchy
   Alt text:    [ ] all images have descriptive alt
   Schema:      [ ] JSON-LD present + valid (if applicable)
   Links:       [ ] internal links where appropriate

   Verdict: ✅ PASS / ⚠️ WARNING / N/A ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

8. SECURITY AUDIT                                        [ ]
   ref: security-headers/SKILL.md
   (forms, API routes, user input, third-party scripts)
   ─────────────────────────────────
   Input:       [ ] server-side Zod validation  [ ] sanitized
   Forms:       [ ] honeypot present  [ ] CSRF origin check
   PII:         [ ] no PII logged to console
   Scripts:     [ ] consent-gated  [ ] CSP-compliant

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL / N/A

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

9. ACCESSIBILITY AUDIT                                   [ ]
   ref: accessibility-a11y/SKILL.md
   ─────────────────────────────────
   Contrast:    [ ] 4.5:1 normal text  [ ] 3:1 large text
   Focus:       [ ] :focus-visible ring on all interactive
   Keyboard:    [ ] Tab order correct  [ ] widgets support keys
   ARIA:        [ ] aria-expanded  [ ] aria-label on icon btns
   Images:      [ ] meaningful alt   [ ] decorative aria-hidden
   Motion:      [ ] prefers-reduced-motion respected

   Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL

   Issues:
   -

   Fixes:
   -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

10. ERROR HANDLING AUDIT                                  [ ]
    ref: error-handling-monitoring/SKILL.md
    (pages, async, dynamic imports, 3D, forms)
    ─────────────────────────────────
    Suspense:    [ ] async wrapped    [ ] dynamic has fallback
    3D/WebGL:    [ ] error boundary   [ ] static fallback
    GSAP:        [ ] try/catch        [ ] gsap.set() not opacity-0
    Forms:       [ ] preserves input  [ ] timeout warning
    API:         [ ] { error, code } shape  [ ] no stack traces

    Verdict: ✅ PASS / ⚠️ WARNING / ❌ FAIL / N/A

    Issues:
    -

    Fixes:
    -

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
FINAL RULING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

   1. BRAND          ✅ / ⚠️ / ❌
   2. ARCHITECTURE   ✅ / ⚠️ / ❌
   3. ANIMATION      ✅ / ⚠️ / ❌
   4. MOBILE         ✅ / ⚠️ / ❌
   5. PERFORMANCE    ✅ / ⚠️ / ❌
   6. COPY           ✅ / ⚠️ / ❌
   7. SEO            ✅ / ⚠️ / N/A / ❌
   8. SECURITY       ✅ / ⚠️ / N/A / ❌
   9. ACCESSIBILITY  ✅ / ⚠️ / ❌
  10. ERROR HANDLING  ✅ / ⚠️ / N/A / ❌

  OVERALL: ✅ APPROVED TO SHIP
        or ❌ REQUIRES FIXES — see issues above

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## Using This Scorecard

1. Copy the block above into a comment on the PR, or into the component's review thread
2. Fill in every `[ ]` checkbox and verdict line
3. Under each section, list specific issues in the format:
   ```
   - [FAIL] src/components/sections/HeroSection.tsx:42 — Image missing `sizes` prop
     Fix: Add sizes="100vw" to the hero <Image> component
   ```
4. Post the completed scorecard. The PR cannot be merged until all `❌ FAIL` items are resolved and re-audited.

## Severity Guide

| Verdict | Meaning | Can ship? |
|---|---|---|
| ✅ PASS | All checks clear | Yes |
| ⚠️ WARNING | Minor issue noted, no user impact | Yes — log for next sprint |
| ❌ FAIL | Clear violation of a Haide standard | No — must fix first |
| N/A | Section not applicable (UI atom, no SEO surface) | Skip section |
