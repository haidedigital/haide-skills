---
name: loading-screens
description: Complete loading and page transition system for the Haide Digital website. Covers the initial page-load loader (branded, 1.2-1.8s), between-page transitions (GSAP, 300ms out/in), React context state machine, FOUC prevention, font synchronisation, and heavy-asset preloading. Use when implementing or modifying the Haide loader component, page transition overlays, LoadingContext, or any first-paint / transition animation on haide.digital.
---

# Loading Screens — Haide Digital

## Design Philosophy

The loader is NOT decoration — it is the site's boot sequence.

- **Tone:** Engineered, precise, not whimsical. Think "system initialising."
- **Duration:** 1.2 – 1.8 s maximum. Never make visitors wait.
- **Minimum display:** 800 ms (prevents flash on fast connections).
- **Transition out:** Curtain rising, NOT a fade. A directional wipe or clip-path reveal.
- **Brand elements:** haide-orange (`#F15928`), the wordmark, or a geometric mark.

---

## State Machine — Read Before Writing Any Code

Always design the state machine first. See `references/state-machine.md` for the complete diagram.

Four loader states:

| State | Trigger | Duration | Behaviour |
|---|---|---|---|
| `booting` | Page mount | 0 ms | CSS visible immediately, no JS dependency |
| `loading` | JS hydrates | 0 – 800 ms | Logo animation plays, assets preload |
| `exiting` | min time elapsed + assets ready | 400 ms | Curtain wipe out |
| `done` | Exit animation complete | — | Loader unmounted, hero reveals, scroll activates |

Visit type affects entry:

| Visit type | Loader behaviour |
|---|---|
| First visit | Full branded loader (all 4 states) |
| Returning visit (sessionStorage flag set) | Skip to `exiting` instantly (200 ms) |
| Between-page navigation | Lightweight `PageTransition` overlay only |
| Error / slow timeout | Graceful fallback — exit after 4 s regardless |

---

## Component Structure

```
src/
  components/
    loader/
      PageLoader.tsx          # Full-screen branded loader (first visit)
      PageTransition.tsx      # Between-page lightweight overlay
      LoaderLogo.tsx          # Animated brand mark ("use client")
      CurtainWipe.tsx         # Reusable exit animation ("use client")
  context/
    LoadingContext.tsx         # Global state + visit detection
  hooks/
    use-loader.ts             # Consume LoadingContext safely
  app/
    layout.tsx                # Mounts <PageLoader> + <PageTransition>
```

---

## Implementation Order

1. **LoadingContext** — state machine + visit detection
2. **CSS loader shell** — visible before JS (prevents FOUC)
3. **LoaderLogo** — brand animation
4. **PageLoader** — full-screen orchestration
5. **CurtainWipe** — reusable exit animation
6. **PageTransition** — between-page overlay
7. Wire into `layout.tsx`

Detailed patterns for each step: see `references/implementation.md`.

---

## LoadingContext (core pattern)

```tsx
// src/context/LoadingContext.tsx
"use client";

import { createContext, useContext, useEffect, useRef, useState } from "react";

type LoaderState = "booting" | "loading" | "exiting" | "done";

interface LoadingContextValue {
  state: LoaderState;
  complete: () => void;       // call when assets are ready
  isFirstVisit: boolean;
}

const LoadingContext = createContext<LoadingContextValue | null>(null);

const VISIT_KEY = "haide_visited";
const MIN_DISPLAY_MS = 800;
const FALLBACK_MS = 4000;

export function LoadingProvider({ children }: { children: React.ReactNode }) {
  const [state, setState] = useState<LoaderState>("booting");
  const isFirstVisit = useRef(
    typeof sessionStorage !== "undefined"
      ? !sessionStorage.getItem(VISIT_KEY)
      : true
  ).current;
  const readyRef = useRef(false);
  const timerRef = useRef<NodeJS.Timeout>();

  useEffect(() => {
    // Mark visit immediately
    sessionStorage.setItem(VISIT_KEY, "1");

    // Returning visitors skip the full loader
    if (!isFirstVisit) {
      setState("exiting");
      timerRef.current = setTimeout(() => setState("done"), 200);
      return;
    }

    setState("loading");

    // Safety fallback — exit regardless after FALLBACK_MS
    const fallback = setTimeout(() => triggerExit(), FALLBACK_MS);

    return () => {
      clearTimeout(timerRef.current);
      clearTimeout(fallback);
    };
  }, []);

  function triggerExit() {
    setState("exiting");
    timerRef.current = setTimeout(() => setState("done"), 400);
  }

  function complete() {
    readyRef.current = true;
    // Respect minimum display time
    setTimeout(() => {
      if (state !== "done") triggerExit();
    }, MIN_DISPLAY_MS);
  }

  return (
    <LoadingContext.Provider value={{ state, complete, isFirstVisit }}>
      {children}
    </LoadingContext.Provider>
  );
}

export function useLoading() {
  const ctx = useContext(LoadingContext);
  if (!ctx) throw new Error("useLoading must be used inside LoadingProvider");
  return ctx;
}
```

---

## FOUC Prevention (CSS Shell)

Add to `src/app/globals.css` — loads before any JS:

```css
/* Loader shell — appears instantly */
#haide-loader {
  position: fixed;
  inset: 0;
  z-index: 9999;
  background: #16232A;             /* haide-dark */
  display: flex;
  align-items: center;
  justify-content: center;
  pointer-events: none;            /* allow clicks through while fading */
}

/* Hide scrollbar during load */
body.loading {
  overflow: hidden;
}
```

Add `className="loading"` to `<body>` in `layout.tsx`, remove it in the `LoadingProvider` when state reaches `done`.

---

## Font Synchronisation

Fonts (Poppins, Noto Sans) are loaded via CSS `@import` in `globals.css`. To prevent font-swap flicker during the loader:

```tsx
// Inside LoadingProvider useEffect, after setState("loading"):
if (document.fonts) {
  document.fonts.ready.then(() => {
    // fonts loaded — mark as one of the completion signals
    fontReadyRef.current = true;
    tryComplete();
  });
}
```

Use a `tryComplete()` function that fires `complete()` only when **both** fonts and assets are ready.

---

## Heavy Asset Preloading (Three.js)

If a Three.js scene (`@/3d/`) is used in the hero, trigger preload during the loader:

```tsx
// Dynamic import during loading state — doesn't render, just fetches bundle
useEffect(() => {
  if (state === "loading") {
    import("@/3d/HeroCanvas").then(() => {
      threeReadyRef.current = true;
      tryComplete();
    });
  }
}, [state]);
```

---

## Page Transition (Between Pages)

Use GSAP for between-page transitions. Full pattern: see `references/page-transitions.md`.

**30-second overview:**
1. `PageTransition` mounts a fixed overlay (`position: fixed; inset: 0; z-index: 9998`)
2. On route change (Next.js App Router): overlay wipes **in** (300 ms GSAP) → scroll resets → page renders → overlay wipes **out** (300 ms)
3. Use `usePathname()` from `next/navigation` to detect route changes
4. Overlay color: `#16232A` (haide-dark)

---

## References

- `references/state-machine.md` — Full state diagram with all transitions
- `references/implementation.md` — Step-by-step implementation patterns for each component
- `references/page-transitions.md` — GSAP page transition patterns + Next.js App Router wiring
