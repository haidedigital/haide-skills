---
name: haide-nextjs-architecture
description: Architectural conventions for the Haide Digital website (haide.digital). Enforces consistent file structure, TypeScript patterns, component organization, server/client boundaries, dynamic import rules, and Tailwind conventions specific to this Next.js 14 App Router project. Use when adding new pages, components, hooks, types, or routes to the Haide website codebase. Prevents architectural drift and ensures every new feature follows the same patterns as the existing codebase.
---

# Haide Next.js Architecture

Stack: **Next.js 14 App Router | TypeScript 5 | Tailwind CSS 3 | shadcn/ui | Framer Motion | Three.js (@react-three/fiber) | pnpm**

> This skill covers Haide-specific architecture only. For general frontend patterns, see `Frontend_Development.md`. For UI aesthetics, see `UI_UX_Pro_Max.md`.

---

## File Structure

```
src/
  app/
    layout.tsx                # Root layout (Navbar + body wrapper)
    page.tsx                  # Home (sections composed in order)
    globals.css               # Tailwind directives + CSS variables
    not-found.tsx             # 404 page
    [route]/
      page.tsx                # Route page component
      layout.tsx              # Route-scoped layout (if needed)
      loading.tsx             # Suspense fallback (if needed)
    api/
      [endpoint]/
        route.ts              # Route handler (NOT .tsx)
  components/
    sections/                 # Full-width page sections (XxxSection.tsx)
    layout/                   # Navbar, Footer, DarkSectionContainer
    ui/                       # shadcn/ui primitives only
    animations/               # Framer Motion wrappers ("use client")
    3d/                       # Three.js / R3F scenes ("use client")
  hooks/                      # Custom hooks (use-kebab-case.ts)
  lib/                        # Shared utilities: cn(), formatters
  types/                      # Shared TypeScript interfaces
public/                       # Static assets
```

**Naming rules:**
- Files: `kebab-case.tsx` / `kebab-case.ts`
- Exported components: PascalCase function names
- Hooks: `use-` prefix (e.g. `use-scroll-progress.ts`)
- API routes: always `route.ts` (never `.tsx`)
- Path alias: `@/*` maps to `src/*` — never use relative `../../` imports

---

## Component Placement

| Directory | Purpose | What does NOT go here |
|---|---|---|
| `sections/` | XxxSection.tsx composed in page.tsx | Reusable sub-components |
| `layout/` | Site scaffolding (Navbar, Footer) | Anything section-specific |
| `ui/` | shadcn/ui generated components | Custom business logic |
| `animations/` | Framer Motion client wrappers | Layout or section logic |
| `3d/` | React Three Fiber scenes | Non-3D content |

Sections are composed top-down in `page.tsx`. Never import one section inside another.

---

## Server vs Client Components

**Default: server component.** Add `"use client"` only when using:
- React hooks (`useState`, `useEffect`, `useRef`, etc.)
- Browser APIs (`window`, `document`, `IntersectionObserver`)
- Event handlers or Framer Motion / Three.js

**Push `"use client"` as deep as possible** — keep sections as server components, extract interactivity into client children in `animations/` or `3d/`:

```tsx
// sections/ServicesSection.tsx (server)
import ServicesTicker from "@/animations/ServicesTicker"; // "use client" inside

export default function ServicesSection() {
  return (
    <section className="...">
      <h2>...</h2>
      <ServicesTicker items={services} />
    </section>
  );
}
```

---

## Dynamic Imports (Framer Motion / Three.js)

Always use dynamic imports with `ssr: false` for heavy animation/3D libraries:

```tsx
import dynamic from "next/dynamic";

// Framer Motion component
const MotionHero = dynamic(() => import("@/animations/MotionHero"), {
  ssr: false,
});

// Three.js / R3F scene
const HeroCanvas = dynamic(() => import("@/3d/HeroCanvas"), {
  ssr: false,
  loading: () => <div className="h-[600px] bg-haide-sand animate-pulse" />,
});
```

All files in `animations/` and `3d/` must have `"use client"` at the top and be imported with `ssr: false` from server components.

---

## TypeScript Conventions

```ts
// Next.js App Router page props
interface PageProps {
  params: { slug: string };
  searchParams: { [key: string]: string | string[] | undefined };
}

// Standard section props
interface SectionProps {
  id?: string;
  className?: string;
}

// Animation wrapper props
interface AnimationProps {
  children: React.ReactNode;
  delay?: number;     // seconds, default 0
  duration?: number;  // seconds, default 0.6
  className?: string;
}
```

**Rules:**
- Always define props as a named interface (never inline)
- Use `React.ReactNode` for children (never `JSX.Element`)
- Avoid `any` — use `unknown` + type guards
- Framer Motion refs: `useRef<HTMLDivElement>(null)`
- Three.js refs: `useRef<THREE.Mesh>(null)`

See `references/typescript-patterns.md` for the full interface library.

---

## Tailwind Design Tokens

**Colors** (defined in `tailwind.config.ts`):

| Token | Hex | Usage |
|---|---|---|
| `bg-haide-sand` | `#E4EEF0` | Light section bg, hero bg |
| `bg-haide-dark` | `#16232A` | Dark section / card backgrounds |
| `text-haide-dark-text` | `#233038` | Headings on light backgrounds |
| `text-haide-orange` | `#F15928` | CTAs, accents, eyebrows, active |

**Typography:**

```tsx
// Eyebrow
<p className="font-poppins font-semibold text-xs text-haide-orange tracking-[0.06em] uppercase">

// H1 (fluid)
<h1 className="font-poppins font-bold text-haide-dark-text"
    style={{ fontSize: "clamp(48px, 8vw, 92px)", lineHeight: "1.01" }}>

// H2 section heading
<h2 className="font-poppins font-bold text-haide-dark-text">
// On dark: text-white

// Body
<p className="font-noto text-[rgba(26,26,26,0.8)]">
// On dark: text-white/80
```

**Dark section rule:** Inside `DarkSectionContainer`, use `text-white` for headings and `text-white/80` for body. Never use `text-haide-dark-text` on dark backgrounds.

**Content wrapper:** Use `max-w-[1560px] mx-auto px-4` for section content.

**Conditional classes:** Always use `cn()` from `@/lib/utils`:
```tsx
import { cn } from "@/lib/utils";
<div className={cn("base", isActive && "active", className)} />
```

**Font loading:** Poppins and Noto Sans are loaded via `@import` in `globals.css`. Apply via `font-poppins` / `font-noto` Tailwind classes. Do NOT use `next/font`.

---

## Image Optimization

Use `next/image` for all raster images. SVGs can be inlined as JSX when they need theming.

```tsx
import Image from "next/image";

<Image
  src="/image.webp"
  alt="Descriptive alt text"
  width={800}
  height={600}
  sizes="(max-width: 768px) 100vw, 50vw"
  priority  // only for hero / LCP images
/>
```

Always specify `width` and `height` to prevent CLS. Use `priority` only above the fold.

---

## Navigation

- Internal links: `import Link from "next/link"` — always
- Hash anchors (same-page): `<a href="#section-id">` — NOT Link
- Give scrollable sections an `id` prop for anchor targeting

---

## References

- `references/typescript-patterns.md` — Full interface library (PageProps, API shapes, Sanity types, animation ref patterns)
