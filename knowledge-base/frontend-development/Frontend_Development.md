---
name: frontend-development
description: Comprehensive frontend development expertise for building premium agency websites, custom CMS implementations, and luxury digital experiences. Covers modern frameworks (React, Vue, Next.js, Nuxt, Astro), performance optimization, accessibility, animations, and headless architecture. Use when building websites, implementing designs, optimizing performance, or creating premium web experiences for $15-50k+ engagements.

trigger_terms_primary:
  - frontend development
  - front-end development
  - HTML
  - CSS
  - JavaScript
  - TypeScript
  - React
  - Vue
  - Next.js
  - Nuxt
  - Astro
  - responsive design
  - mobile-first
  - UI development
  - web development
  - website development
  - component development
  - web components

trigger_terms_contextual:
  - build website
  - create website
  - develop site
  - component architecture
  - design system
  - page speed
  - performance optimization
  - Core Web Vitals
  - implement design
  - code the design
  - turn design into code
  - frontend framework
  - CSS architecture
  - JavaScript framework

trigger_terms_client_language:
  - our website needs updating
  - make it look modern
  - we need a new website
  - premium feel
  - interactive elements
  - animations on the site
  - smooth scrolling
  - the site is slow
  - mobile version looks bad
  - need a CMS
  - easy to update content
  - looks cheap needs to look premium

trigger_terms_premium:
  - luxury website
  - premium web experience
  - enterprise frontend
  - high-end development
  - boutique agency website
  - white-glove frontend
  - executive-level website
  - $50k+ website build

related_skills:
  required_foundation:
    - web-ui-ux-design
  frequently_used_with:
    - strategic-technical-seo
    - branding-expert
    - visual-content-direction
  advanced_integration:
    - javascript-seo
    - automation-systems-design
    - data-measurement

premium_context: true
complexity_level: advanced
---

# Frontend Development Skill

## Philosophy

**Frontend development is the bridge between design vision and user experience—where pixels become interactions and static mockups become living, breathing digital products.**

Premium frontend execution separates commodity websites from luxury digital experiences. The difference between a $5k website and a $50k website isn't just design—it's the craftsmanship of implementation: the butter-smooth animations, the instant load times, the accessibility that works for everyone, and the attention to detail that clients feel but can't articulate.

In boutique agency context, frontend development is a strategic differentiator. While volume agencies outsource to the cheapest developers, premium agencies invest in A-player frontend talent who understand that every millisecond of load time, every micro-interaction, and every pixel of spacing contributes to the perception of quality that justifies premium pricing.

---

## When to Use This Skill

### Activate This Skill When:

**Primary Triggers:**
- Building a new website or web application from scratch
- Implementing a design into functional code
- Choosing between frontend frameworks (React, Vue, Next.js, etc.)
- Optimizing website performance (Core Web Vitals, page speed)
- Creating or implementing a design system
- Adding animations, interactions, or motion design
- Integrating with a CMS (headless or traditional)
- Building responsive layouts for all devices
- Ensuring accessibility compliance (WCAG 2.1 AA/AAA)

**Premium Context Triggers:**
- Building websites for $15-50k+ engagements
- Enterprise-scale frontend architecture decisions
- Luxury brand website implementations
- High-stakes website launches for premium clients
- Agency website builds that must demonstrate expertise
- Performance-critical applications where speed equals revenue

**Client Language Triggers:**
- "Our website needs to feel more premium"
- "We want animations like [competitor]"
- "The site needs to be fast on mobile"
- "We need a CMS so we can update content"
- "Make sure it works for everyone"
- "It needs to look modern and professional"

### Do NOT Use This Skill When:
- Pure design work without implementation (use Web/UI/UX Design)
- SEO strategy without technical implementation (use Strategic Technical SEO)
- Content creation (use Editorial & Copywriting or Content Strategy)
- Backend API development (outside scope—focus on consumption not creation)

---

## Core Competencies

### 1. Semantic HTML & Accessibility

**What This Is:**
The foundation of every premium website—semantic HTML that machines understand and accessibility that ensures everyone can use your site. This isn't optional; it's the baseline for professional frontend work.

**Premium Application:**
Premium clients increasingly face accessibility lawsuits. Delivering WCAG 2.1 AA-compliant websites protects clients legally while demonstrating expertise that justifies higher pricing. Enterprise clients often mandate accessibility compliance.

#### Semantic HTML Elements

```html
<!-- Premium semantic structure -->
<header role="banner">
  <nav role="navigation" aria-label="Main navigation">
    <!-- Navigation content -->
  </nav>
</header>

<main role="main" id="main-content">
  <article>
    <header>
      <h1>Page Title</h1>
      <time datetime="2026-02-07">February 7, 2026</time>
    </header>
    <section aria-labelledby="section-heading">
      <h2 id="section-heading">Section Title</h2>
      <!-- Section content -->
    </section>
  </article>
  <aside role="complementary">
    <!-- Sidebar content -->
  </aside>
</main>

<footer role="contentinfo">
  <!-- Footer content -->
</footer>
```

#### Semantic Elements Reference

| Element | Purpose | SEO/Accessibility Benefit |
|---------|---------|---------------------------|
| `<header>` | Introductory content, navigation | Defines page/section header |
| `<nav>` | Navigation links | Screen readers announce navigation |
| `<main>` | Primary content | Skip-to-content target |
| `<article>` | Self-contained content | Syndication-ready, clear content boundaries |
| `<section>` | Thematic grouping | Content organization signals |
| `<aside>` | Related but separate content | Distinguishes secondary content |
| `<footer>` | Footer content | Defines page/section footer |
| `<figure>` / `<figcaption>` | Images with captions | Proper image-caption relationship |
| `<time>` | Dates/times | Machine-readable timestamps |
| `<address>` | Contact information | Structured contact data |

#### Accessibility Implementation (WCAG 2.1 AA)

**Keyboard Navigation Checklist:**
- [ ] All interactive elements focusable via Tab key
- [ ] Visible focus indicators (`:focus-visible` styling)
- [ ] Skip link to main content (`<a href="#main-content" class="skip-link">`)
- [ ] Logical tab order matching visual order
- [ ] No keyboard traps
- [ ] Escape key closes modals/overlays
- [ ] Arrow keys navigate within components (menus, tabs)

**Color Contrast Requirements:**
- Normal text: 4.5:1 minimum contrast ratio
- Large text (18pt+ or bold 14pt+): 3:1 minimum ratio
- UI components: 3:1 minimum ratio
- Tools: WebAIM Contrast Checker, Axe DevTools

**ARIA Implementation:**

```html
<!-- Button with state -->
<button
  aria-expanded="false"
  aria-controls="menu-content"
  aria-haspopup="menu"
>
  Menu
</button>

<!-- Live region for dynamic content -->
<div aria-live="polite" aria-atomic="true">
  <!-- Announcements appear here -->
</div>

<!-- Modal dialog -->
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-description"
>
  <h2 id="dialog-title">Dialog Title</h2>
  <p id="dialog-description">Dialog description</p>
</div>

<!-- Form with accessible labels -->
<form>
  <label for="email">Email Address</label>
  <input
    type="email"
    id="email"
    name="email"
    aria-required="true"
    aria-describedby="email-hint email-error"
  />
  <span id="email-hint" class="hint">We'll never share your email</span>
  <span id="email-error" class="error" role="alert"></span>
</form>
```

**Reduced Motion Support:**

```css
/* Respect user preference for reduced motion */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

---

### 2. CSS Architecture & Styling

**What This Is:**
The systematic approach to writing, organizing, and scaling CSS across projects. Poor CSS architecture leads to unmaintainable code, specificity wars, and inconsistent implementations.

**Premium Application:**
Premium websites require consistent visual systems that scale. Design tokens and CSS custom properties enable brand consistency, easy theming (dark mode), and maintainable code that future developers can understand.

#### CSS Architecture Approaches

| Approach | Best For | Trade-offs |
|----------|----------|------------|
| **BEM** | Custom control, legacy projects | Verbose class names, manual discipline |
| **Tailwind CSS** | Rapid development, consistency | Cluttered HTML, learning curve |
| **CSS Modules** | Component-based (React/Vue) | Build tooling required |
| **styled-components** | Dynamic theming, React | JS bundle size, runtime cost |
| **Design Tokens** | Multi-platform, enterprise | Setup complexity |

#### CSS Custom Properties System

```css
/* Base design tokens */
:root {
  /* Colors - Semantic naming */
  --color-primary: #0066cc;
  --color-primary-hover: #0052a3;
  --color-secondary: #6c757d;
  --color-surface: #ffffff;
  --color-surface-elevated: #f8f9fa;
  --color-text: #1a1a1a;
  --color-text-muted: #6c757d;
  --color-border: #dee2e6;
  --color-error: #dc3545;
  --color-success: #28a745;

  /* Premium palette - Soft neutrals */
  --color-bg-premium: #f9f7f3;
  --color-text-premium: #2d2d2d;
  --color-accent-premium: #8b7355;

  /* Typography scale - Fluid */
  --text-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
  --text-sm: clamp(0.875rem, 0.8rem + 0.375vw, 1rem);
  --text-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
  --text-lg: clamp(1.125rem, 1rem + 0.625vw, 1.25rem);
  --text-xl: clamp(1.25rem, 1rem + 1.25vw, 1.5rem);
  --text-2xl: clamp(1.5rem, 1rem + 2.5vw, 2rem);
  --text-3xl: clamp(1.875rem, 1rem + 4.375vw, 3rem);
  --text-4xl: clamp(2.25rem, 1rem + 6.25vw, 4rem);
  --text-5xl: clamp(3rem, 1rem + 10vw, 6rem);

  /* Spacing scale */
  --space-xs: 0.25rem;   /* 4px */
  --space-sm: 0.5rem;    /* 8px */
  --space-md: 1rem;      /* 16px */
  --space-lg: 1.5rem;    /* 24px */
  --space-xl: 2rem;      /* 32px */
  --space-2xl: 3rem;     /* 48px */
  --space-3xl: 4rem;     /* 64px */
  --space-4xl: 6rem;     /* 96px */
  --space-5xl: 8rem;     /* 128px */

  /* Motion */
  --duration-fast: 150ms;
  --duration-normal: 300ms;
  --duration-slow: 500ms;
  --ease-out: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-in-out: cubic-bezier(0.65, 0, 0.35, 1);

  /* Shadows - Premium depth */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.07);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.1);
  --shadow-xl: 0 20px 25px rgba(0, 0, 0, 0.1);

  /* Border radius */
  --radius-sm: 0.25rem;
  --radius-md: 0.5rem;
  --radius-lg: 1rem;
  --radius-full: 9999px;
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
  :root {
    --color-surface: #1a1a1a;
    --color-surface-elevated: #2d2d2d;
    --color-text: #f5f5f5;
    --color-text-muted: #a0a0a0;
    --color-border: #404040;
  }
}

/* Manual theme override */
.theme-dark {
  --color-surface: #1a1a1a;
  --color-surface-elevated: #2d2d2d;
  --color-text: #f5f5f5;
}

.theme-light {
  --color-surface: #ffffff;
  --color-surface-elevated: #f8f9fa;
  --color-text: #1a1a1a;
}
```

#### Fluid Typography with CSS Clamp

```css
/* The formula: clamp(min, preferred, max) */

/* Headings */
h1 {
  font-size: var(--text-5xl);
  line-height: 1.1;
  letter-spacing: -0.02em;
}

h2 {
  font-size: var(--text-4xl);
  line-height: 1.2;
  letter-spacing: -0.01em;
}

h3 {
  font-size: var(--text-3xl);
  line-height: 1.3;
}

/* Body text */
body {
  font-size: var(--text-base);
  line-height: 1.6;
}

/* Premium typography - Extra large hero text */
.hero-headline {
  font-size: clamp(2.5rem, 1rem + 8vw, 7rem);
  font-weight: 700;
  line-height: 1;
  letter-spacing: -0.03em;
}
```

#### BEM Naming Convention

```css
/* Block */
.card { }

/* Element (part of block) */
.card__header { }
.card__title { }
.card__body { }
.card__footer { }
.card__image { }

/* Modifier (variation) */
.card--featured { }
.card--compact { }
.card__title--large { }

/* Example usage */
.card {
  background: var(--color-surface);
  border-radius: var(--radius-lg);
  box-shadow: var(--shadow-md);
  overflow: hidden;
}

.card__header {
  padding: var(--space-lg);
  border-bottom: 1px solid var(--color-border);
}

.card__title {
  font-size: var(--text-xl);
  font-weight: 600;
  margin: 0;
}

.card__body {
  padding: var(--space-lg);
}

.card--featured {
  border: 2px solid var(--color-primary);
}

.card--featured .card__title {
  color: var(--color-primary);
}
```

#### Tailwind CSS Patterns

```html
<!-- Card component with Tailwind -->
<article class="bg-white rounded-2xl shadow-lg overflow-hidden hover:shadow-xl transition-shadow duration-300">
  <div class="p-6 border-b border-gray-100">
    <h3 class="text-xl font-semibold text-gray-900">Card Title</h3>
  </div>
  <div class="p-6">
    <p class="text-gray-600 leading-relaxed">Card content goes here.</p>
  </div>
</article>

<!-- Premium button with Tailwind -->
<button class="
  px-8 py-4
  bg-gray-900 text-white font-medium
  rounded-full
  hover:bg-gray-800
  focus:outline-none focus:ring-2 focus:ring-gray-900 focus:ring-offset-2
  transition-all duration-200
  active:scale-95
">
  Get Started
</button>

<!-- Responsive grid -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 lg:gap-8">
  <!-- Grid items -->
</div>
```

---

### 3. JavaScript & TypeScript Excellence

**What This Is:**
Modern JavaScript development using ES6+ features and TypeScript for type safety. This competency covers the language fundamentals that underpin framework development.

**Premium Application:**
TypeScript is standard in premium development—catching errors early, improving IDE support, and creating self-documenting code. Premium clients expect codebases that future developers can maintain.

#### Essential ES6+ Features

```typescript
// Destructuring
const { name, email, role = 'user' } = user;
const [first, second, ...rest] = items;

// Spread operator
const newUser = { ...user, updatedAt: new Date() };
const allItems = [...oldItems, ...newItems];

// Template literals
const greeting = `Hello, ${user.name}! You have ${count} notifications.`;

// Arrow functions
const double = (n: number): number => n * 2;
const users = data.map(item => item.user);

// Async/await
async function fetchUser(id: string): Promise<User> {
  try {
    const response = await fetch(`/api/users/${id}`);
    if (!response.ok) throw new Error('User not found');
    return response.json();
  } catch (error) {
    console.error('Fetch failed:', error);
    throw error;
  }
}

// Optional chaining and nullish coalescing
const userName = user?.profile?.name ?? 'Anonymous';
const avatar = user?.avatar || '/default-avatar.png';

// Array methods
const activeUsers = users
  .filter(user => user.isActive)
  .map(user => ({ id: user.id, name: user.name }))
  .sort((a, b) => a.name.localeCompare(b.name));

// Object methods
const entries = Object.entries(config);
const values = Object.values(settings);
const merged = Object.assign({}, defaults, overrides);
```

#### TypeScript Patterns

```typescript
// Type definitions
interface User {
  id: string;
  name: string;
  email: string;
  role: 'admin' | 'user' | 'guest';
  createdAt: Date;
  profile?: UserProfile;
}

interface UserProfile {
  bio: string;
  avatar: string;
  social: {
    twitter?: string;
    linkedin?: string;
  };
}

// Generic types
interface ApiResponse<T> {
  data: T;
  status: number;
  message: string;
}

async function fetchData<T>(url: string): Promise<ApiResponse<T>> {
  const response = await fetch(url);
  return response.json();
}

// Type guards
function isUser(value: unknown): value is User {
  return (
    typeof value === 'object' &&
    value !== null &&
    'id' in value &&
    'email' in value
  );
}

// Utility types
type PartialUser = Partial<User>;
type RequiredUser = Required<User>;
type ReadonlyUser = Readonly<User>;
type UserKeys = keyof User;
type UserName = Pick<User, 'id' | 'name'>;
type UserWithoutEmail = Omit<User, 'email'>;

// Component props pattern (React)
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'ghost';
  size?: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

// Discriminated unions
type NotificationState =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success'; data: Notification[] }
  | { status: 'error'; error: Error };
```

#### Module Patterns

```typescript
// Named exports (preferred)
// utils/formatting.ts
export function formatCurrency(amount: number, currency = 'USD'): string {
  return new Intl.NumberFormat('en-US', {
    style: 'currency',
    currency,
  }).format(amount);
}

export function formatDate(date: Date, locale = 'en-US'): string {
  return new Intl.DateTimeFormat(locale, {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
  }).format(date);
}

// Barrel exports
// utils/index.ts
export * from './formatting';
export * from './validation';
export * from './helpers';

// Usage
import { formatCurrency, formatDate } from '@/utils';
```

---

### 4. Framework Selection & Implementation

**What This Is:**
Choosing and implementing the right JavaScript framework for the project. Each framework has strengths—the skill is matching framework to requirements.

**Premium Application:**
Premium clients pay for informed decisions, not framework preferences. The right choice depends on team expertise, project requirements, and long-term maintainability. Premium agencies document the rationale.

#### Framework Decision Matrix

| Factor | React | Vue | Next.js | Nuxt | Astro |
|--------|-------|-----|---------|------|-------|
| **Best For** | Complex SPAs | Content sites | SEO-focused React | SEO-focused Vue | Content-heavy static |
| **Learning Curve** | Moderate | Lower | Moderate | Lower | Low |
| **SSR/SSG** | Manual setup | Manual setup | Built-in | Built-in | Built-in |
| **Bundle Size** | Medium | Small | Medium | Medium | Minimal |
| **Enterprise Adoption** | Highest | Growing | Growing | Growing | Emerging |
| **Job Market** | Largest | Strong | Growing | Moderate | Niche |
| **Ecosystem** | Massive | Strong | Large | Strong | Growing |

#### When to Choose Each Framework

**Choose React When:**
- Team has React expertise
- Complex, user-driven interfaces (dashboards, apps)
- Large ecosystem requirements
- Hiring considerations (largest talent pool)
- Integration with React Native needed

**Choose Vue When:**
- Team has Vue expertise or is new to frameworks
- Content-focused projects
- Faster initial development timeline
- Prefer convention over configuration
- Smaller bundle size matters

**Choose Next.js When:**
- React team needs SSR/SSG
- SEO is critical (organic traffic matters)
- Image optimization required
- API routes needed
- Vercel deployment (optimal experience)

**Choose Nuxt When:**
- Vue team needs SSR/SSG
- Value convention over configuration
- Content-focused with SEO requirements
- Faster setup preferred
- Auto-import and file-based routing valued

**Choose Astro When:**
- Content-heavy static sites
- Minimal JavaScript required
- Performance is paramount
- Multi-framework islands needed
- Blog, documentation, marketing sites

#### Rendering Strategy Matrix

| Strategy | Best For | SEO | Performance | Complexity |
|----------|----------|-----|-------------|------------|
| **SSG (Static)** | Marketing sites, blogs | Excellent | Excellent | Low |
| **SSR** | Dynamic content, personalization | Excellent | Good | Medium |
| **ISR** | E-commerce, frequently updated | Excellent | Excellent | Medium |
| **CSR (SPA)** | Dashboards, apps | Poor | Medium | Low |
| **Hybrid** | Complex requirements | Excellent | Excellent | High |

```typescript
// Next.js - SSG (Static Site Generation)
export async function getStaticProps() {
  const posts = await fetchPosts();
  return {
    props: { posts },
    revalidate: 60, // ISR: regenerate every 60 seconds
  };
}

// Next.js - SSR (Server-Side Rendering)
export async function getServerSideProps(context) {
  const user = await fetchUser(context.req.cookies.token);
  return {
    props: { user },
  };
}

// Nuxt 3 - Hybrid rendering
export default defineNuxtConfig({
  routeRules: {
    '/': { prerender: true },           // SSG
    '/blog/**': { swr: 3600 },          // ISR (1 hour)
    '/dashboard/**': { ssr: false },    // CSR
    '/api/**': { cors: true },          // API routes
  },
});
```

#### Component Architecture

```tsx
// React component with TypeScript
interface CardProps {
  title: string;
  description: string;
  image?: string;
  href: string;
  variant?: 'default' | 'featured';
}

export function Card({
  title,
  description,
  image,
  href,
  variant = 'default'
}: CardProps) {
  return (
    <article className={cn(
      'rounded-2xl overflow-hidden transition-shadow',
      variant === 'featured'
        ? 'shadow-xl ring-2 ring-primary'
        : 'shadow-md hover:shadow-lg'
    )}>
      {image && (
        <Image
          src={image}
          alt=""
          width={400}
          height={300}
          className="w-full h-48 object-cover"
        />
      )}
      <div className="p-6">
        <h3 className="text-xl font-semibold mb-2">{title}</h3>
        <p className="text-gray-600 mb-4">{description}</p>
        <Link
          href={href}
          className="text-primary font-medium hover:underline"
        >
          Learn more
        </Link>
      </div>
    </article>
  );
}
```

```vue
<!-- Vue 3 Composition API component -->
<script setup lang="ts">
interface Props {
  title: string;
  description: string;
  image?: string;
  href: string;
  variant?: 'default' | 'featured';
}

const props = withDefaults(defineProps<Props>(), {
  variant: 'default',
});

const cardClasses = computed(() => [
  'rounded-2xl overflow-hidden transition-shadow',
  props.variant === 'featured'
    ? 'shadow-xl ring-2 ring-primary'
    : 'shadow-md hover:shadow-lg',
]);
</script>

<template>
  <article :class="cardClasses">
    <img
      v-if="image"
      :src="image"
      alt=""
      class="w-full h-48 object-cover"
    />
    <div class="p-6">
      <h3 class="text-xl font-semibold mb-2">{{ title }}</h3>
      <p class="text-gray-600 mb-4">{{ description }}</p>
      <NuxtLink :to="href" class="text-primary font-medium hover:underline">
        Learn more
      </NuxtLink>
    </div>
  </article>
</template>
```

---

### 5. Performance Optimization

**What This Is:**
Optimizing website performance for Core Web Vitals, user experience, and business metrics. Performance directly impacts conversions, SEO rankings, and user satisfaction.

**Premium Application:**
Premium websites must perform in the top 10% of the industry. Every 100ms of delay costs conversions. Performance optimization is a key differentiator that justifies premium pricing.

#### Core Web Vitals Targets (2026)

| Metric | Target | Measures | Primary Factors |
|--------|--------|----------|-----------------|
| **LCP** | < 2.5s | Loading | Largest image/text, server response, render-blocking resources |
| **INP** | < 200ms | Interactivity | JavaScript execution, event handlers, main thread blocking |
| **CLS** | < 0.1 | Visual Stability | Unsized images, dynamic content, font loading |

#### Image Optimization

```tsx
// Next.js Image component (automatic optimization)
import Image from 'next/image';

<Image
  src="/hero.jpg"
  alt="Hero image"
  width={1920}
  height={1080}
  priority  // LCP optimization - load immediately
  placeholder="blur"
  blurDataURL={blurDataUrl}
/>

// Responsive images with art direction
<picture>
  <source
    media="(min-width: 1024px)"
    srcSet="/hero-desktop.webp"
    type="image/webp"
  />
  <source
    media="(min-width: 640px)"
    srcSet="/hero-tablet.webp"
    type="image/webp"
  />
  <source
    srcSet="/hero-mobile.webp"
    type="image/webp"
  />
  <img
    src="/hero-fallback.jpg"
    alt="Hero image"
    loading="lazy"
    decoding="async"
  />
</picture>
```

**Image Format Selection:**

| Format | Best For | Browser Support |
|--------|----------|-----------------|
| **AVIF** | All images (best compression) | Modern browsers |
| **WebP** | All images (good fallback) | Wide support |
| **PNG** | Transparency, lossless | Universal |
| **JPEG** | Photographs (legacy fallback) | Universal |
| **SVG** | Icons, logos, illustrations | Universal |

#### Code Splitting & Lazy Loading

```tsx
// Dynamic imports (Next.js)
import dynamic from 'next/dynamic';

// Lazy load heavy component
const HeavyChart = dynamic(() => import('@/components/HeavyChart'), {
  loading: () => <ChartSkeleton />,
  ssr: false, // Client-side only
});

// Lazy load below-fold components
const Testimonials = dynamic(() => import('@/components/Testimonials'));
const FAQ = dynamic(() => import('@/components/FAQ'));

// React.lazy with Suspense
const Modal = lazy(() => import('./Modal'));

function App() {
  return (
    <Suspense fallback={<ModalSkeleton />}>
      <Modal />
    </Suspense>
  );
}
```

#### Font Optimization

```css
/* Preload critical fonts */
<link
  rel="preload"
  href="/fonts/inter-var.woff2"
  as="font"
  type="font/woff2"
  crossorigin
/>

/* Font-display for performance */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter-var.woff2') format('woff2');
  font-weight: 100 900;
  font-style: normal;
  font-display: swap; /* Show fallback immediately, swap when loaded */
}

/* Reduce CLS with font fallback matching */
@font-face {
  font-family: 'Inter';
  src: url('/fonts/inter-var.woff2') format('woff2');
  font-display: swap;
  size-adjust: 100%;
  ascent-override: 90%;
  descent-override: 20%;
  line-gap-override: 0%;
}
```

#### JavaScript Performance

```typescript
// Debounce expensive operations
function debounce<T extends (...args: any[]) => void>(
  fn: T,
  delay: number
): (...args: Parameters<T>) => void {
  let timeoutId: NodeJS.Timeout;
  return (...args) => {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => fn(...args), delay);
  };
}

// Throttle scroll handlers
function throttle<T extends (...args: any[]) => void>(
  fn: T,
  limit: number
): (...args: Parameters<T>) => void {
  let inThrottle = false;
  return (...args) => {
    if (!inThrottle) {
      fn(...args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}

// Usage
const handleScroll = throttle(() => {
  // Expensive scroll calculation
}, 100);

window.addEventListener('scroll', handleScroll, { passive: true });
```

#### Caching Strategy

```typescript
// Service Worker caching (Workbox)
import { precacheAndRoute } from 'workbox-precaching';
import { registerRoute } from 'workbox-routing';
import { CacheFirst, StaleWhileRevalidate } from 'workbox-strategies';

// Precache static assets
precacheAndRoute(self.__WB_MANIFEST);

// Cache images
registerRoute(
  ({ request }) => request.destination === 'image',
  new CacheFirst({
    cacheName: 'images',
    plugins: [
      new ExpirationPlugin({
        maxEntries: 60,
        maxAgeSeconds: 30 * 24 * 60 * 60, // 30 days
      }),
    ],
  })
);

// Cache API responses
registerRoute(
  ({ url }) => url.pathname.startsWith('/api/'),
  new StaleWhileRevalidate({
    cacheName: 'api-cache',
  })
);
```

---

### 6. Premium Design Implementation

**What This Is:**
Implementing animations, micro-interactions, and motion design that elevate websites from functional to exceptional. This is what separates premium websites from commodity templates.

**Premium Application:**
Premium clients expect butter-smooth animations, thoughtful micro-interactions, and a level of polish that competitors can't match. This is where $50k websites differentiate from $5k websites.

#### Animation Libraries Comparison

| Library | Best For | Size | Learning Curve |
|---------|----------|------|----------------|
| **GSAP** | Complex scroll animations, timelines | 60KB | Moderate |
| **Framer Motion** | React animations, gestures | 30KB | Low |
| **Lenis** | Smooth scrolling only | 3KB | Very Low |
| **CSS Scroll-Driven** | Native scroll animations | 0KB | Low |

#### GSAP ScrollTrigger Implementation

```typescript
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

// Fade in on scroll
gsap.from('.fade-in', {
  scrollTrigger: {
    trigger: '.fade-in',
    start: 'top 80%',
    toggleActions: 'play none none reverse',
  },
  opacity: 0,
  y: 50,
  duration: 0.8,
  ease: 'power2.out',
});

// Parallax effect
gsap.to('.parallax-bg', {
  scrollTrigger: {
    trigger: '.parallax-section',
    start: 'top bottom',
    end: 'bottom top',
    scrub: true,
  },
  y: -200,
  ease: 'none',
});

// Staggered card reveal
gsap.from('.card', {
  scrollTrigger: {
    trigger: '.cards-container',
    start: 'top 70%',
  },
  opacity: 0,
  y: 100,
  stagger: 0.1,
  duration: 0.6,
  ease: 'power2.out',
});

// Text reveal animation
const splitText = new SplitText('.hero-title', { type: 'chars' });
gsap.from(splitText.chars, {
  scrollTrigger: {
    trigger: '.hero-title',
    start: 'top 80%',
  },
  opacity: 0,
  y: 50,
  rotateX: -90,
  stagger: 0.02,
  duration: 0.5,
  ease: 'back.out(1.7)',
});
```

#### Framer Motion (React)

```tsx
import { motion, AnimatePresence } from 'framer-motion';

// Page transition wrapper
export function PageTransition({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 20 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -20 }}
      transition={{ duration: 0.3, ease: 'easeOut' }}
    >
      {children}
    </motion.div>
  );
}

// Staggered list animation
const containerVariants = {
  hidden: { opacity: 0 },
  visible: {
    opacity: 1,
    transition: {
      staggerChildren: 0.1,
    },
  },
};

const itemVariants = {
  hidden: { opacity: 0, y: 20 },
  visible: { opacity: 1, y: 0 },
};

export function AnimatedList({ items }: { items: Item[] }) {
  return (
    <motion.ul
      variants={containerVariants}
      initial="hidden"
      whileInView="visible"
      viewport={{ once: true, margin: '-100px' }}
    >
      {items.map((item) => (
        <motion.li key={item.id} variants={itemVariants}>
          {item.content}
        </motion.li>
      ))}
    </motion.ul>
  );
}

// Interactive card with hover
export function InteractiveCard({ children }: { children: React.ReactNode }) {
  return (
    <motion.div
      whileHover={{
        scale: 1.02,
        boxShadow: '0 20px 25px rgba(0, 0, 0, 0.15)',
      }}
      whileTap={{ scale: 0.98 }}
      transition={{ type: 'spring', stiffness: 300, damping: 20 }}
      className="bg-white rounded-2xl p-6"
    >
      {children}
    </motion.div>
  );
}
```

#### Smooth Scrolling with Lenis

```typescript
import Lenis from '@studio-freight/lenis';

// Initialize Lenis
const lenis = new Lenis({
  duration: 1.2,
  easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)),
  direction: 'vertical',
  gestureDirection: 'vertical',
  smooth: true,
  smoothTouch: false,
  touchMultiplier: 2,
});

// Animation loop
function raf(time: number) {
  lenis.raf(time);
  requestAnimationFrame(raf);
}

requestAnimationFrame(raf);

// Sync with GSAP ScrollTrigger
lenis.on('scroll', ScrollTrigger.update);

gsap.ticker.add((time) => {
  lenis.raf(time * 1000);
});

gsap.ticker.lagSmoothing(0);

// Scroll to element
function scrollToSection(id: string) {
  lenis.scrollTo(`#${id}`, {
    offset: -100,
    duration: 1.5,
  });
}
```

#### CSS Micro-Interactions

```css
/* Button hover with ripple effect */
.btn-premium {
  position: relative;
  overflow: hidden;
  background: var(--color-primary);
  color: white;
  padding: 1rem 2rem;
  border-radius: var(--radius-full);
  transition: transform 0.2s, box-shadow 0.2s;
}

.btn-premium:hover {
  transform: translateY(-2px);
  box-shadow: 0 10px 20px rgba(0, 102, 204, 0.3);
}

.btn-premium:active {
  transform: translateY(0);
}

.btn-premium::after {
  content: '';
  position: absolute;
  inset: 0;
  background: radial-gradient(circle, rgba(255,255,255,0.3) 0%, transparent 70%);
  opacity: 0;
  transform: scale(0);
  transition: opacity 0.3s, transform 0.3s;
}

.btn-premium:hover::after {
  opacity: 1;
  transform: scale(2);
}

/* Link hover underline animation */
.link-fancy {
  position: relative;
  text-decoration: none;
}

.link-fancy::after {
  content: '';
  position: absolute;
  left: 0;
  bottom: -2px;
  width: 100%;
  height: 2px;
  background: currentColor;
  transform: scaleX(0);
  transform-origin: right;
  transition: transform 0.3s ease;
}

.link-fancy:hover::after {
  transform: scaleX(1);
  transform-origin: left;
}

/* Card image zoom on hover */
.card-image-container {
  overflow: hidden;
}

.card-image {
  transition: transform 0.5s ease;
}

.card:hover .card-image {
  transform: scale(1.05);
}

/* Form input focus state */
.input-premium {
  border: 2px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: 1rem;
  transition: border-color 0.2s, box-shadow 0.2s;
}

.input-premium:focus {
  outline: none;
  border-color: var(--color-primary);
  box-shadow: 0 0 0 4px rgba(0, 102, 204, 0.1);
}
```

#### Custom Cursor Implementation

```typescript
// Custom cursor with CursorJS
import Cursor from 'cursorjs';

const cursor = new Cursor({
  container: 'body',
  cursor: '.custom-cursor',
  cursorInner: '.cursor-inner',
  cursorOuter: '.cursor-outer',
});

// Custom cursor with vanilla JS
class PremiumCursor {
  cursor: HTMLElement;
  cursorDot: HTMLElement;

  constructor() {
    this.cursor = document.querySelector('.cursor')!;
    this.cursorDot = document.querySelector('.cursor-dot')!;
    this.init();
  }

  init() {
    document.addEventListener('mousemove', (e) => {
      gsap.to(this.cursor, {
        x: e.clientX,
        y: e.clientY,
        duration: 0.5,
        ease: 'power2.out',
      });

      gsap.to(this.cursorDot, {
        x: e.clientX,
        y: e.clientY,
        duration: 0.1,
      });
    });

    // Expand on interactive elements
    document.querySelectorAll('a, button').forEach((el) => {
      el.addEventListener('mouseenter', () => {
        gsap.to(this.cursor, { scale: 1.5, duration: 0.3 });
      });

      el.addEventListener('mouseleave', () => {
        gsap.to(this.cursor, { scale: 1, duration: 0.3 });
      });
    });
  }
}
```

---

### 7. CMS Integration & Headless Architecture

**What This Is:**
Integrating content management systems for client content updates. Includes traditional CMS (WordPress) and modern headless options (Sanity, Contentful, Strapi).

**Premium Application:**
Premium clients need intuitive content management that non-technical team members can use. The CMS choice impacts long-term maintenance costs and client satisfaction.

#### CMS Selection Matrix

| CMS | Best For | Pricing | Customization | Dev Experience |
|-----|----------|---------|---------------|----------------|
| **Sanity** | Custom content models, real-time | Free tier + usage | Excellent | Excellent |
| **Contentful** | Enterprise, multi-channel | $489+/mo | Good | Good |
| **Strapi** | Self-hosted, SQL backend | Free (self-host) | Excellent | Good |
| **WordPress** | Familiar editors, plugins | Hosting cost | Limited | Moderate |
| **Prismic** | Visual editor, slices | Free tier + $150/mo | Good | Good |

#### Sanity.io Integration

```typescript
// sanity.config.ts
import { defineConfig } from 'sanity';
import { deskTool } from 'sanity/desk';
import { visionTool } from '@sanity/vision';

export default defineConfig({
  name: 'default',
  title: 'My Premium Website',
  projectId: 'your-project-id',
  dataset: 'production',
  plugins: [deskTool(), visionTool()],
  schema: {
    types: [
      // Page schema
      {
        name: 'page',
        title: 'Page',
        type: 'document',
        fields: [
          {
            name: 'title',
            title: 'Title',
            type: 'string',
            validation: (Rule) => Rule.required(),
          },
          {
            name: 'slug',
            title: 'Slug',
            type: 'slug',
            options: { source: 'title' },
          },
          {
            name: 'content',
            title: 'Content',
            type: 'array',
            of: [
              { type: 'hero' },
              { type: 'textSection' },
              { type: 'imageGallery' },
              { type: 'testimonials' },
              { type: 'cta' },
            ],
          },
          {
            name: 'seo',
            title: 'SEO',
            type: 'seo',
          },
        ],
      },

      // Hero component
      {
        name: 'hero',
        title: 'Hero Section',
        type: 'object',
        fields: [
          { name: 'headline', type: 'string' },
          { name: 'subheadline', type: 'text' },
          { name: 'backgroundImage', type: 'image' },
          { name: 'cta', type: 'cta' },
        ],
      },
    ],
  },
});
```

```typescript
// Fetching content with GROQ
import { createClient } from '@sanity/client';

const client = createClient({
  projectId: 'your-project-id',
  dataset: 'production',
  useCdn: true,
  apiVersion: '2024-01-01',
});

// Fetch page by slug
export async function getPageBySlug(slug: string) {
  return client.fetch(
    `*[_type == "page" && slug.current == $slug][0]{
      title,
      slug,
      content[]{
        _type,
        _type == "hero" => {
          headline,
          subheadline,
          "backgroundImage": backgroundImage.asset->url,
          cta
        },
        _type == "textSection" => {
          heading,
          body
        }
      },
      seo
    }`,
    { slug }
  );
}

// Real-time preview
import { useLiveQuery } from '@sanity/preview-kit';

export function PreviewPage({ slug }: { slug: string }) {
  const [data] = useLiveQuery(null, pageQuery, { slug });
  return <PageComponent data={data} />;
}
```

#### WordPress Headless Integration

```typescript
// WordPress REST API integration
const WP_API_URL = 'https://your-site.com/wp-json/wp/v2';

interface WPPost {
  id: number;
  title: { rendered: string };
  content: { rendered: string };
  excerpt: { rendered: string };
  slug: string;
  featured_media: number;
  acf?: Record<string, any>; // ACF fields
}

export async function getPosts(): Promise<WPPost[]> {
  const response = await fetch(`${WP_API_URL}/posts?_embed`);
  return response.json();
}

export async function getPostBySlug(slug: string): Promise<WPPost> {
  const response = await fetch(`${WP_API_URL}/posts?slug=${slug}&_embed`);
  const posts = await response.json();
  return posts[0];
}

// With ACF fields
export async function getPageWithACF(slug: string) {
  const response = await fetch(
    `${WP_API_URL}/pages?slug=${slug}&acf_format=standard`
  );
  const pages = await response.json();
  return pages[0];
}

// GraphQL with WPGraphQL plugin
import { gql } from '@apollo/client';

const GET_POST = gql`
  query GetPost($slug: ID!) {
    post(id: $slug, idType: SLUG) {
      title
      content
      featuredImage {
        node {
          sourceUrl
          altText
        }
      }
      seo {
        title
        metaDesc
        opengraphImage {
          sourceUrl
        }
      }
    }
  }
`;
```

#### Content Modeling Best Practices

```typescript
// Flexible page builder pattern
interface PageBlock {
  _type: string;
  _key: string;
}

interface HeroBlock extends PageBlock {
  _type: 'hero';
  headline: string;
  subheadline?: string;
  backgroundImage: Image;
  cta?: CTA;
  variant: 'centered' | 'left-aligned' | 'split';
}

interface TextBlock extends PageBlock {
  _type: 'textSection';
  heading: string;
  body: PortableTextBlock[];
  alignment: 'left' | 'center';
}

interface TestimonialsBlock extends PageBlock {
  _type: 'testimonials';
  heading: string;
  testimonials: Testimonial[];
  layout: 'carousel' | 'grid';
}

// Render blocks dynamically
const blockComponents: Record<string, React.ComponentType<any>> = {
  hero: HeroSection,
  textSection: TextSection,
  testimonials: TestimonialsSection,
  imageGallery: ImageGallery,
  cta: CTASection,
};

function PageBuilder({ blocks }: { blocks: PageBlock[] }) {
  return (
    <>
      {blocks.map((block) => {
        const Component = blockComponents[block._type];
        if (!Component) return null;
        return <Component key={block._key} {...block} />;
      })}
    </>
  );
}
```

---

## Strategic Framework: The Premium Frontend System

### Phase 1: Discovery & Architecture (Week 1)

**Objective:** Understand requirements and establish technical foundation.

**Activities:**
- [ ] Review design files (Figma, Sketch, Adobe XD)
- [ ] Identify component patterns and create component inventory
- [ ] Choose framework based on decision matrix
- [ ] Choose CSS architecture approach
- [ ] Define design tokens and CSS custom properties
- [ ] Plan CMS integration (if applicable)
- [ ] Set up project structure and tooling
- [ ] Establish coding standards and conventions

**Deliverables:**
- Technical specification document
- Component inventory
- Project scaffolding with tooling configured
- Design tokens defined

### Phase 2: Foundation Build (Weeks 2-3)

**Objective:** Build core infrastructure and base components.

**Activities:**
- [ ] Implement design system (tokens, base styles)
- [ ] Build foundational components (buttons, inputs, cards, typography)
- [ ] Set up routing and page structure
- [ ] Implement responsive layout system
- [ ] Configure CMS (if applicable)
- [ ] Set up testing infrastructure
- [ ] Implement accessibility foundations

**Deliverables:**
- Component library (Storybook or equivalent)
- Base page templates
- CMS schema and configuration
- Automated testing setup

### Phase 3: Feature Build (Weeks 3-5)

**Objective:** Build all page sections and features.

**Activities:**
- [ ] Build all page sections (hero, content blocks, etc.)
- [ ] Implement navigation and footer
- [ ] Build dynamic pages (blog, portfolio, etc.)
- [ ] Integrate CMS content
- [ ] Implement forms and interactions
- [ ] Add state management (if needed)
- [ ] Cross-browser testing

**Deliverables:**
- All pages functional
- CMS integration complete
- Forms working
- Basic responsiveness verified

### Phase 4: Polish & Animation (Week 5-6)

**Objective:** Add premium polish and motion design.

**Activities:**
- [ ] Implement scroll animations (GSAP, Framer Motion)
- [ ] Add micro-interactions
- [ ] Implement page transitions
- [ ] Add loading states and skeleton screens
- [ ] Polish responsive behavior
- [ ] Implement dark mode (if specified)
- [ ] Add custom cursor (if specified)

**Deliverables:**
- All animations implemented
- Premium polish applied
- Motion respects `prefers-reduced-motion`

### Phase 5: Optimization & Launch (Week 6-7)

**Objective:** Optimize performance and prepare for launch.

**Activities:**
- [ ] Performance audit (Lighthouse, WebPageTest)
- [ ] Image optimization and lazy loading
- [ ] Code splitting and bundle optimization
- [ ] Accessibility audit (Axe, WAVE)
- [ ] SEO verification (meta tags, structured data)
- [ ] Cross-device testing
- [ ] Load testing
- [ ] Launch checklist completion

**Deliverables:**
- Lighthouse scores 90+ (all metrics)
- WCAG 2.1 AA compliant
- Production deployment
- Documentation

---

## Premium Application

### White-Glove Frontend Delivery Standards

**Premium Website Performance Benchmarks:**

| Metric | Standard | Premium | Elite |
|--------|----------|---------|-------|
| Lighthouse Performance | 80+ | 90+ | 95+ |
| LCP | < 3s | < 2.5s | < 1.5s |
| INP | < 300ms | < 200ms | < 100ms |
| CLS | < 0.15 | < 0.1 | < 0.05 |
| Time to Interactive | < 5s | < 3.5s | < 2.5s |

**Premium Code Quality Standards:**

```
✅ TypeScript with strict mode
✅ ESLint + Prettier configured
✅ Component documentation (Storybook or equivalent)
✅ Unit test coverage > 80% for utilities
✅ E2E tests for critical paths
✅ Accessibility automated testing
✅ Visual regression testing for design system
✅ No console errors or warnings
✅ No TypeScript errors
✅ Code review required for all merges
```

**Premium Delivery Checklist:**

- [ ] All Lighthouse scores 90+
- [ ] WCAG 2.1 AA compliant (verified with automated + manual testing)
- [ ] Works on latest 2 versions of Chrome, Firefox, Safari, Edge
- [ ] Responsive from 320px to 4K displays
- [ ] Touch-friendly on mobile (44x44px minimum touch targets)
- [ ] Fast 3G performance acceptable (< 10s load)
- [ ] Print stylesheet (if applicable)
- [ ] Favicon set (all sizes)
- [ ] Open Graph and Twitter cards configured
- [ ] Structured data implemented
- [ ] 404 page designed and implemented
- [ ] Form validation with accessible error messages
- [ ] Loading states for all async operations
- [ ] Error boundaries for graceful failure

### Enterprise Frontend Patterns

**Monorepo Structure for Large Projects:**

```
apps/
├── web/                 # Main website
├── admin/               # Admin dashboard
├── docs/                # Documentation site
packages/
├── ui/                  # Shared component library
├── config/              # Shared configs (ESLint, TypeScript)
├── utils/               # Shared utilities
├── types/               # Shared TypeScript types
```

**Component Library Standards:**

```typescript
// Every component should have:
// 1. TypeScript interface for props
interface ButtonProps {
  variant: 'primary' | 'secondary' | 'ghost';
  size: 'sm' | 'md' | 'lg';
  disabled?: boolean;
  loading?: boolean;
  children: React.ReactNode;
  onClick?: () => void;
}

// 2. Default props
const defaultProps: Partial<ButtonProps> = {
  variant: 'primary',
  size: 'md',
  disabled: false,
  loading: false,
};

// 3. Prop documentation
/**
 * Primary button component for user interactions.
 *
 * @param variant - Visual style variant
 * @param size - Button size
 * @param disabled - Whether button is disabled
 * @param loading - Shows loading spinner when true
 */
export function Button(props: ButtonProps) { }

// 4. Storybook story
export default {
  title: 'Components/Button',
  component: Button,
  argTypes: {
    variant: {
      control: { type: 'select' },
      options: ['primary', 'secondary', 'ghost'],
    },
  },
};
```

---

## Detailed Methodologies

### Methodology 1: Premium Website Build Process

**Phase 1: Setup (Day 1)**

```bash
# Create Next.js project with TypeScript
npx create-next-app@latest my-project --typescript --tailwind --app

# Add essential dependencies
npm install framer-motion @studio-freight/lenis
npm install -D @types/node prettier eslint-config-prettier

# Setup project structure
mkdir -p src/{components,lib,hooks,types,styles}
mkdir -p src/components/{ui,layout,sections}
```

**Phase 2: Design System (Days 2-3)**

```typescript
// src/styles/tokens.css
:root {
  /* Define all design tokens from Figma/design file */
}

// src/lib/utils.ts
import { clsx, type ClassValue } from 'clsx';
import { twMerge } from 'tailwind-merge';

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}

// src/components/ui/button.tsx
// Build base components with variants
```

**Phase 3: Layout & Navigation (Days 4-5)**

```typescript
// src/components/layout/header.tsx
// src/components/layout/footer.tsx
// src/components/layout/mobile-nav.tsx
// src/app/layout.tsx
```

**Phase 4: Page Sections (Days 6-12)**

```typescript
// Build all section components
// src/components/sections/hero.tsx
// src/components/sections/features.tsx
// src/components/sections/testimonials.tsx
// etc.
```

**Phase 5: CMS Integration (Days 13-15)**

```typescript
// Setup CMS client
// Create content types/schemas
// Build dynamic pages
// Test preview mode
```

**Phase 6: Animation & Polish (Days 16-18)**

```typescript
// Add scroll animations
// Implement micro-interactions
// Add loading states
// Polish responsive behavior
```

**Phase 7: Optimization & Launch (Days 19-21)**

```bash
# Performance audit
npx lighthouse https://staging.example.com --output=html

# Accessibility audit
npx axe https://staging.example.com

# Bundle analysis
npm run build -- --analyze
```

---

### Methodology 2: Design System Implementation

**Step 1: Extract Design Tokens**

From Figma/design file, extract:
- Colors (with semantic naming)
- Typography scale (with fluid values)
- Spacing scale
- Border radii
- Shadows
- Breakpoints

**Step 2: Create Token Structure**

```css
/* globals.css */
@layer base {
  :root {
    /* Colors */
    --color-brand-primary: 220 90% 45%;
    --color-brand-secondary: 35 95% 50%;

    /* Surfaces */
    --color-surface-primary: 0 0% 100%;
    --color-surface-secondary: 220 15% 97%;

    /* Text */
    --color-text-primary: 220 20% 10%;
    --color-text-secondary: 220 10% 40%;

    /* ... all tokens */
  }
}
```

**Step 3: Configure Tailwind**

```typescript
// tailwind.config.ts
import type { Config } from 'tailwindcss';

const config: Config = {
  content: ['./src/**/*.{ts,tsx}'],
  theme: {
    extend: {
      colors: {
        brand: {
          primary: 'hsl(var(--color-brand-primary) / <alpha-value>)',
          secondary: 'hsl(var(--color-brand-secondary) / <alpha-value>)',
        },
        surface: {
          primary: 'hsl(var(--color-surface-primary) / <alpha-value>)',
          secondary: 'hsl(var(--color-surface-secondary) / <alpha-value>)',
        },
      },
      fontSize: {
        // Fluid type scale
        'display-lg': ['clamp(3rem, 1rem + 8vw, 6rem)', { lineHeight: '1.1' }],
        'display': ['clamp(2.5rem, 1rem + 6vw, 4.5rem)', { lineHeight: '1.1' }],
        // ... etc
      },
    },
  },
};
```

**Step 4: Build Component Library**

Start with atoms → molecules → organisms:

```
components/
├── ui/              # Atoms
│   ├── button.tsx
│   ├── input.tsx
│   ├── badge.tsx
│   └── avatar.tsx
├── patterns/        # Molecules
│   ├── card.tsx
│   ├── form-field.tsx
│   └── nav-link.tsx
└── sections/        # Organisms
    ├── hero.tsx
    ├── features.tsx
    └── testimonials.tsx
```

---

### Methodology 3: Performance Optimization Protocol

**Step 1: Baseline Measurement**

```bash
# Run Lighthouse CI
npx lighthouse https://example.com --output=json --output-path=./baseline.json

# WebPageTest for detailed waterfall
# https://www.webpagetest.org/

# Real User Monitoring setup (Vercel Analytics, Core Web Vitals)
```

**Step 2: Image Optimization**

```typescript
// next.config.js
module.exports = {
  images: {
    formats: ['image/avif', 'image/webp'],
    deviceSizes: [640, 750, 828, 1080, 1200, 1920, 2048, 3840],
    imageSizes: [16, 32, 48, 64, 96, 128, 256, 384],
    minimumCacheTTL: 60 * 60 * 24 * 365, // 1 year
  },
};
```

**Step 3: JavaScript Optimization**

```typescript
// Dynamic imports for heavy components
const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <Skeleton />,
});

// Tree-shake unused code
import { specificFunction } from 'large-library';
// NOT: import * as library from 'large-library';
```

**Step 4: Font Optimization**

```typescript
// next/font for automatic optimization
import { Inter } from 'next/font/google';

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
});
```

**Step 5: Third-Party Script Management**

```typescript
// Load non-critical scripts after hydration
import Script from 'next/script';

<Script
  src="https://analytics.example.com/script.js"
  strategy="lazyOnload"
/>

// Or use Partytown for web workers
import { Partytown } from '@builder.io/partytown/react';
```

**Step 6: Verify Improvements**

```bash
# Compare to baseline
npx lighthouse https://example.com --output=json --output-path=./after.json

# Budget check
npx bundlesize
```

---

### Methodology 4: Animation & Motion Design System

**Step 1: Define Motion Principles**

```typescript
// lib/motion.ts
export const durations = {
  fast: 0.15,
  normal: 0.3,
  slow: 0.5,
  slower: 0.8,
};

export const easings = {
  easeOut: [0.16, 1, 0.3, 1],
  easeInOut: [0.65, 0, 0.35, 1],
  spring: { type: 'spring', stiffness: 300, damping: 30 },
};

export const fadeIn = {
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  exit: { opacity: 0 },
  transition: { duration: durations.normal },
};

export const slideUp = {
  initial: { opacity: 0, y: 20 },
  animate: { opacity: 1, y: 0 },
  exit: { opacity: 0, y: -20 },
  transition: { duration: durations.normal, ease: easings.easeOut },
};

export const stagger = {
  animate: {
    transition: {
      staggerChildren: 0.1,
    },
  },
};
```

**Step 2: Create Animation Components**

```tsx
// components/motion/fade-in.tsx
'use client';

import { motion } from 'framer-motion';
import { fadeIn } from '@/lib/motion';

interface FadeInProps {
  children: React.ReactNode;
  delay?: number;
  className?: string;
}

export function FadeIn({ children, delay = 0, className }: FadeInProps) {
  return (
    <motion.div
      {...fadeIn}
      transition={{ ...fadeIn.transition, delay }}
      className={className}
    >
      {children}
    </motion.div>
  );
}
```

**Step 3: Implement Scroll Animations**

```tsx
// components/motion/scroll-reveal.tsx
'use client';

import { motion, useInView } from 'framer-motion';
import { useRef } from 'react';

interface ScrollRevealProps {
  children: React.ReactNode;
  className?: string;
}

export function ScrollReveal({ children, className }: ScrollRevealProps) {
  const ref = useRef(null);
  const isInView = useInView(ref, { once: true, margin: '-100px' });

  return (
    <motion.div
      ref={ref}
      initial={{ opacity: 0, y: 50 }}
      animate={isInView ? { opacity: 1, y: 0 } : { opacity: 0, y: 50 }}
      transition={{ duration: 0.6, ease: [0.16, 1, 0.3, 1] }}
      className={className}
    >
      {children}
    </motion.div>
  );
}
```

**Step 4: Respect User Preferences**

```tsx
// hooks/use-reduced-motion.ts
import { useEffect, useState } from 'react';

export function useReducedMotion() {
  const [reducedMotion, setReducedMotion] = useState(false);

  useEffect(() => {
    const mediaQuery = window.matchMedia('(prefers-reduced-motion: reduce)');
    setReducedMotion(mediaQuery.matches);

    const handler = (e: MediaQueryListEvent) => setReducedMotion(e.matches);
    mediaQuery.addEventListener('change', handler);
    return () => mediaQuery.removeEventListener('change', handler);
  }, []);

  return reducedMotion;
}

// Usage in components
function AnimatedComponent() {
  const reducedMotion = useReducedMotion();

  return (
    <motion.div
      animate={{ x: 100 }}
      transition={{
        duration: reducedMotion ? 0 : 0.5,
      }}
    />
  );
}
```

---

### Methodology 5: Headless CMS Integration

**Step 1: Choose CMS Based on Requirements**

| Requirement | Recommended CMS |
|-------------|-----------------|
| Real-time collaboration | Sanity |
| Enterprise compliance | Contentful |
| Self-hosted requirement | Strapi |
| Familiar to client | WordPress (headless) |
| Visual editing | Prismic, Storyblok |

**Step 2: Define Content Model**

```typescript
// Example: Sanity schema for agency website
// schemas/page.ts
export default {
  name: 'page',
  title: 'Page',
  type: 'document',
  fields: [
    { name: 'title', type: 'string', validation: (Rule) => Rule.required() },
    { name: 'slug', type: 'slug', options: { source: 'title' } },
    {
      name: 'sections',
      type: 'array',
      of: [
        { type: 'heroSection' },
        { type: 'servicesGrid' },
        { type: 'caseStudyCarousel' },
        { type: 'teamSection' },
        { type: 'testimonialSection' },
        { type: 'ctaSection' },
        { type: 'faqSection' },
      ],
    },
    { name: 'seo', type: 'seoFields' },
  ],
};
```

**Step 3: Create API Layer**

```typescript
// lib/sanity/client.ts
import { createClient } from '@sanity/client';

export const client = createClient({
  projectId: process.env.SANITY_PROJECT_ID!,
  dataset: process.env.SANITY_DATASET!,
  useCdn: process.env.NODE_ENV === 'production',
  apiVersion: '2024-01-01',
});

// lib/sanity/queries.ts
export const pageBySlugQuery = `
  *[_type == "page" && slug.current == $slug][0]{
    title,
    sections[]{
      _type,
      _key,
      ...
    },
    seo
  }
`;

// lib/sanity/fetch.ts
export async function getPageBySlug(slug: string) {
  return client.fetch(pageBySlugQuery, { slug });
}
```

**Step 4: Build Dynamic Pages**

```tsx
// app/[slug]/page.tsx
import { getPageBySlug } from '@/lib/sanity/fetch';
import { SectionRenderer } from '@/components/section-renderer';
import { notFound } from 'next/navigation';

export default async function Page({ params }: { params: { slug: string } }) {
  const page = await getPageBySlug(params.slug);

  if (!page) notFound();

  return (
    <main>
      {page.sections.map((section) => (
        <SectionRenderer key={section._key} section={section} />
      ))}
    </main>
  );
}

// Generate static paths
export async function generateStaticParams() {
  const slugs = await client.fetch(`*[_type == "page"].slug.current`);
  return slugs.map((slug: string) => ({ slug }));
}
```

**Step 5: Implement Preview Mode**

```typescript
// app/api/preview/route.ts
import { draftMode } from 'next/headers';
import { redirect } from 'next/navigation';

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url);
  const secret = searchParams.get('secret');
  const slug = searchParams.get('slug');

  if (secret !== process.env.SANITY_PREVIEW_SECRET) {
    return new Response('Invalid token', { status: 401 });
  }

  draftMode().enable();
  redirect(`/${slug}`);
}
```

---

### Methodology 6: Responsive Implementation Strategy

**Step 1: Define Breakpoints**

```typescript
// tailwind.config.ts
export default {
  theme: {
    screens: {
      'sm': '640px',   // Mobile landscape
      'md': '768px',   // Tablet portrait
      'lg': '1024px',  // Tablet landscape / small desktop
      'xl': '1280px',  // Desktop
      '2xl': '1536px', // Large desktop
    },
  },
};
```

**Step 2: Mobile-First Implementation**

```tsx
// Always start with mobile styles, then add larger breakpoints
<div className="
  px-4          // Mobile: 16px padding
  md:px-8       // Tablet: 32px padding
  lg:px-16      // Desktop: 64px padding
  xl:px-24      // Large: 96px padding

  grid
  grid-cols-1   // Mobile: 1 column
  md:grid-cols-2 // Tablet: 2 columns
  lg:grid-cols-3 // Desktop: 3 columns

  gap-4         // Mobile: 16px gap
  md:gap-6      // Tablet: 24px gap
  lg:gap-8      // Desktop: 32px gap
">
  {/* Content */}
</div>
```

**Step 3: Container Query Components**

```css
/* For component-level responsiveness */
@container (min-width: 400px) {
  .card {
    flex-direction: row;
  }
}
```

```tsx
<div className="@container">
  <article className="@md:flex-row flex flex-col">
    {/* Responds to container width, not viewport */}
  </article>
</div>
```

**Step 4: Touch-Friendly Interactions**

```css
/* Minimum touch target size */
.touch-target {
  min-height: 44px;
  min-width: 44px;
}

/* Larger tap areas on mobile */
@media (pointer: coarse) {
  .btn {
    padding: 1rem 1.5rem;
  }

  .nav-link {
    padding: 0.75rem 1rem;
  }
}
```

**Step 5: Responsive Images**

```tsx
// Different images for different viewports (art direction)
<picture>
  <source
    media="(min-width: 1024px)"
    srcSet="/hero-desktop.jpg"
  />
  <source
    media="(min-width: 640px)"
    srcSet="/hero-tablet.jpg"
  />
  <img
    src="/hero-mobile.jpg"
    alt="Hero"
    className="w-full h-auto"
  />
</picture>

// Next.js responsive image
<Image
  src="/hero.jpg"
  alt="Hero"
  fill
  sizes="(max-width: 768px) 100vw, (max-width: 1200px) 50vw, 33vw"
  className="object-cover"
/>
```

---

## Decision Frameworks

### Framework Selection Decision Tree

```
START: What type of project?
│
├─► Static content site (blog, docs, marketing)?
│   └─► Astro (minimal JS, best performance)
│
├─► SEO-critical with React team?
│   └─► Next.js (SSG/SSR, image optimization)
│
├─► SEO-critical with Vue team?
│   └─► Nuxt (SSG/SSR, auto-imports)
│
├─► Complex SPA (dashboard, app)?
│   ├─► Team knows React? → React + Vite
│   └─► Team knows Vue? → Vue + Vite
│
└─► E-commerce?
    ├─► Shopify/headless? → Next.js or Nuxt
    └─► Custom? → Next.js + Medusa/Saleor
```

### CSS Architecture Decision Tree

```
START: Team size and project scope?
│
├─► Solo dev, rapid prototyping?
│   └─► Tailwind CSS (fastest iteration)
│
├─► Small team, custom design system?
│   └─► CSS Modules + design tokens
│
├─► React app with complex theming?
│   └─► styled-components or Tailwind
│
├─► Large team, strict conventions?
│   └─► BEM with design tokens
│
└─► Multi-platform (web + native)?
    └─► Design tokens (platform-agnostic)
```

### CMS Selection Decision Tree

```
START: Who manages content?
│
├─► Marketing team (non-technical)?
│   ├─► Need visual editing? → Prismic, Storyblok
│   └─► Familiar with WordPress? → WordPress headless
│
├─► Content team with schema needs?
│   └─► Sanity (custom schemas, real-time)
│
├─► Enterprise with compliance?
│   └─► Contentful (enterprise features)
│
├─► Must self-host?
│   └─► Strapi (open-source, SQL)
│
└─► Simple blog/docs?
    └─► MDX files (no CMS needed)
```

### Animation Library Selection

```
START: What animations needed?
│
├─► Basic hover/click interactions?
│   └─► CSS transitions only
│
├─► Scroll-triggered animations?
│   ├─► Simple fade/slide → Framer Motion
│   └─► Complex timelines → GSAP ScrollTrigger
│
├─► Smooth scrolling only?
│   └─► Lenis (3KB, lightweight)
│
├─► Page transitions (React)?
│   └─► Framer Motion (AnimatePresence)
│
├─► SVG path animations?
│   └─► Framer Motion or GSAP
│
└─► 3D/WebGL?
    └─► Three.js / React Three Fiber
```

---

## Common Scenarios

### Scenario 1: Building Premium Agency Website from Scratch

**Context:** Client wants a $40k agency website showcasing their services, team, and case studies. SEO is important for lead generation.

**Approach:**

1. **Framework:** Next.js (SSG for SEO, React ecosystem)
2. **Styling:** Tailwind CSS + CSS custom properties for design tokens
3. **CMS:** Sanity (flexible content modeling, real-time preview)
4. **Animations:** GSAP ScrollTrigger + Lenis for smooth scrolling
5. **Hosting:** Vercel (optimal Next.js experience)

**Key Implementation Points:**
- Typography-first hero with scroll-triggered text animations
- Case study pages with full-bleed imagery and parallax
- Team section with subtle hover animations
- Client logo grid with grayscale → color hover
- Contact form with inline validation and loading states
- Dark mode toggle with system preference detection

**Quality Gates:**
- Lighthouse 95+ all metrics
- WCAG 2.1 AA compliant
- < 2s LCP on mobile
- All animations respect reduced motion preference

---

### Scenario 2: Migrating from WordPress to Headless CMS

**Context:** Client's WordPress site is slow and hard to maintain. They want modern performance with familiar content editing.

**Approach:**

1. **Content Migration:**
   - Export WordPress content via REST API or WXR export
   - Transform to new CMS schema
   - Migrate media assets to CDN

2. **New Stack:**
   - Next.js for frontend
   - Sanity for CMS (visual editing familiar to WordPress users)
   - Vercel for hosting

3. **Migration Steps:**
   ```
   Week 1: Content audit and schema design
   Week 2: CMS setup and content migration
   Week 3: Frontend development (templates)
   Week 4: Design polish and animations
   Week 5: Testing and launch
   ```

4. **SEO Preservation:**
   - 1:1 URL mapping
   - 301 redirects for changed URLs
   - Structured data migration
   - Sitemap generation

---

### Scenario 3: Implementing Dark Mode

**Context:** Existing light-mode website needs dark mode support.

**Implementation:**

```typescript
// 1. Update CSS custom properties
:root {
  --color-bg: #ffffff;
  --color-text: #1a1a1a;
}

[data-theme="dark"] {
  --color-bg: #1a1a1a;
  --color-text: #f5f5f5;
}

// 2. Create theme toggle hook
function useTheme() {
  const [theme, setTheme] = useState<'light' | 'dark'>('light');

  useEffect(() => {
    // Check system preference
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    // Check stored preference
    const stored = localStorage.getItem('theme') as 'light' | 'dark' | null;

    setTheme(stored || (prefersDark ? 'dark' : 'light'));
  }, []);

  useEffect(() => {
    document.documentElement.setAttribute('data-theme', theme);
    localStorage.setItem('theme', theme);
  }, [theme]);

  const toggle = () => setTheme(t => t === 'light' ? 'dark' : 'light');

  return { theme, toggle };
}

// 3. Toggle button component
function ThemeToggle() {
  const { theme, toggle } = useTheme();

  return (
    <button
      onClick={toggle}
      aria-label={`Switch to ${theme === 'light' ? 'dark' : 'light'} mode`}
    >
      {theme === 'light' ? <MoonIcon /> : <SunIcon />}
    </button>
  );
}
```

---

### Scenario 4: Adding Scroll Animations to Static Site

**Context:** Marketing site feels flat. Add premium scroll animations without rebuild.

**Implementation:**

```typescript
// 1. Add GSAP via CDN or npm
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/ScrollTrigger.min.js"></script>

// 2. Initialize animations
gsap.registerPlugin(ScrollTrigger);

// Fade in all sections
gsap.utils.toArray('.section').forEach((section) => {
  gsap.from(section, {
    scrollTrigger: {
      trigger: section,
      start: 'top 80%',
      toggleActions: 'play none none reverse',
    },
    opacity: 0,
    y: 50,
    duration: 0.8,
  });
});

// Stagger cards
gsap.utils.toArray('.card-grid').forEach((grid) => {
  gsap.from(grid.querySelectorAll('.card'), {
    scrollTrigger: {
      trigger: grid,
      start: 'top 70%',
    },
    opacity: 0,
    y: 30,
    stagger: 0.1,
    duration: 0.5,
  });
});

// 3. Add smooth scrolling
const lenis = new Lenis();
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
```

---

### Scenario 5: Optimizing Core Web Vitals on React SPA

**Context:** React SPA has poor Core Web Vitals. Client losing SEO rankings.

**Diagnosis and Fix:**

1. **LCP Issues:**
   ```typescript
   // Problem: Large hero image not optimized
   // Fix: Use Next.js Image with priority
   <Image src="/hero.jpg" priority fill sizes="100vw" />

   // Problem: Render-blocking CSS
   // Fix: Critical CSS inline, rest async
   ```

2. **INP Issues:**
   ```typescript
   // Problem: Heavy JavaScript blocking main thread
   // Fix: Code split and lazy load
   const HeavyComponent = lazy(() => import('./Heavy'));

   // Problem: Expensive event handlers
   // Fix: Debounce/throttle
   const handleSearch = useDeferredValue(searchTerm);
   ```

3. **CLS Issues:**
   ```typescript
   // Problem: Unsized images
   // Fix: Always specify dimensions
   <Image width={800} height={600} ... />

   // Problem: Font loading causes shift
   // Fix: Font display swap with matched fallback
   font-display: swap;
   ```

4. **Migration Path:**
   ```
   If staying React SPA:
   → Add SSR with Next.js (best option)
   → Or add prerendering with react-snap

   If can rebuild:
   → Next.js with App Router
   → SSG for static pages, SSR for dynamic
   ```

---

### Scenario 6: Creating Responsive Design System

**Context:** Team building multiple sites needs consistent, responsive component library.

**Implementation:**

1. **Token Structure:**
   ```typescript
   // tokens/index.ts
   export const breakpoints = {
     sm: '640px',
     md: '768px',
     lg: '1024px',
     xl: '1280px',
   };

   export const spacing = {
     0: '0',
     1: '0.25rem',
     2: '0.5rem',
     // ... responsive variants
   };

   export const typography = {
     'heading-1': {
       fontSize: 'clamp(2rem, 1rem + 4vw, 4rem)',
       lineHeight: 1.1,
       fontWeight: 700,
     },
   };
   ```

2. **Responsive Component Pattern:**
   ```tsx
   interface CardProps {
     variant?: 'vertical' | 'horizontal' | 'responsive';
   }

   function Card({ variant = 'responsive' }: CardProps) {
     return (
       <article className={cn(
         'rounded-lg overflow-hidden',
         variant === 'vertical' && 'flex-col',
         variant === 'horizontal' && 'flex-row',
         variant === 'responsive' && 'flex-col md:flex-row',
       )}>
         {/* ... */}
       </article>
     );
   }
   ```

3. **Storybook with Viewport Testing:**
   ```typescript
   // .storybook/preview.ts
   export const parameters = {
     viewport: {
       viewports: {
         mobile: { name: 'Mobile', styles: { width: '375px', height: '667px' } },
         tablet: { name: 'Tablet', styles: { width: '768px', height: '1024px' } },
         desktop: { name: 'Desktop', styles: { width: '1440px', height: '900px' } },
       },
     },
   };
   ```

---

## Premium UI Patterns for Agencies

### Hero Section (2026 Trends)

```tsx
// Typography-first hero with scroll animation
function PremiumHero() {
  return (
    <section className="min-h-screen flex items-center relative overflow-hidden">
      {/* Background with parallax */}
      <motion.div
        className="absolute inset-0 z-0"
        style={{ y: useTransform(scrollY, [0, 500], [0, 150]) }}
      >
        <Image src="/hero-bg.jpg" fill className="object-cover opacity-20" />
      </motion.div>

      {/* Content */}
      <div className="container mx-auto px-4 relative z-10">
        <motion.h1
          className="text-[clamp(3rem,10vw,8rem)] font-bold leading-none"
          initial={{ opacity: 0, y: 100 }}
          animate={{ opacity: 1, y: 0 }}
          transition={{ duration: 0.8, ease: [0.16, 1, 0.3, 1] }}
        >
          We build
          <br />
          <span className="text-brand">digital experiences</span>
        </motion.h1>

        <motion.p
          className="text-xl md:text-2xl mt-8 max-w-xl"
          initial={{ opacity: 0 }}
          animate={{ opacity: 1 }}
          transition={{ delay: 0.3 }}
        >
          Premium digital agency crafting websites that convert.
        </motion.p>
      </div>
    </section>
  );
}
```

### Case Study Card

```tsx
function CaseStudyCard({ study }: { study: CaseStudy }) {
  return (
    <motion.article
      className="group relative overflow-hidden rounded-2xl"
      whileHover={{ scale: 1.02 }}
      transition={{ duration: 0.3 }}
    >
      {/* Image with zoom on hover */}
      <div className="aspect-[4/3] overflow-hidden">
        <Image
          src={study.image}
          alt={study.title}
          fill
          className="object-cover transition-transform duration-700 group-hover:scale-105"
        />
      </div>

      {/* Overlay */}
      <div className="absolute inset-0 bg-gradient-to-t from-black/80 via-black/20 to-transparent" />

      {/* Content */}
      <div className="absolute bottom-0 left-0 right-0 p-8">
        <span className="text-sm text-white/70 uppercase tracking-wider">
          {study.category}
        </span>
        <h3 className="text-2xl font-bold text-white mt-2">{study.title}</h3>
        <p className="text-white/80 mt-2 line-clamp-2">{study.excerpt}</p>

        {/* Metrics */}
        <div className="flex gap-6 mt-4">
          {study.metrics.map((metric) => (
            <div key={metric.label}>
              <div className="text-2xl font-bold text-white">{metric.value}</div>
              <div className="text-sm text-white/60">{metric.label}</div>
            </div>
          ))}
        </div>
      </div>
    </motion.article>
  );
}
```

### Client Logos Grid

```tsx
function ClientLogos({ clients }: { clients: Client[] }) {
  return (
    <section className="py-20 bg-surface-secondary">
      <div className="container mx-auto px-4">
        <motion.h2
          className="text-center text-sm uppercase tracking-wider text-muted mb-12"
          initial={{ opacity: 0 }}
          whileInView={{ opacity: 1 }}
          viewport={{ once: true }}
        >
          Trusted by industry leaders
        </motion.h2>

        <motion.div
          className="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-6 gap-8 items-center"
          initial="hidden"
          whileInView="visible"
          viewport={{ once: true }}
          variants={{
            visible: { transition: { staggerChildren: 0.1 } },
          }}
        >
          {clients.map((client) => (
            <motion.div
              key={client.name}
              className="flex items-center justify-center"
              variants={{
                hidden: { opacity: 0, y: 20 },
                visible: { opacity: 1, y: 0 },
              }}
            >
              <Image
                src={client.logo}
                alt={client.name}
                width={120}
                height={40}
                className="grayscale hover:grayscale-0 opacity-50 hover:opacity-100 transition-all duration-300"
              />
            </motion.div>
          ))}
        </motion.div>
      </div>
    </section>
  );
}
```

### Testimonial Carousel

```tsx
function TestimonialCarousel({ testimonials }: { testimonials: Testimonial[] }) {
  const [current, setCurrent] = useState(0);

  return (
    <section className="py-24 bg-surface">
      <div className="container mx-auto px-4 max-w-4xl">
        <AnimatePresence mode="wait">
          <motion.blockquote
            key={current}
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            exit={{ opacity: 0, y: -20 }}
            className="text-center"
          >
            <p className="text-2xl md:text-3xl font-light leading-relaxed">
              "{testimonials[current].quote}"
            </p>

            <footer className="mt-8 flex items-center justify-center gap-4">
              <Image
                src={testimonials[current].avatar}
                alt={testimonials[current].author}
                width={48}
                height={48}
                className="rounded-full"
              />
              <div className="text-left">
                <cite className="font-semibold not-italic">
                  {testimonials[current].author}
                </cite>
                <p className="text-sm text-muted">
                  {testimonials[current].role}, {testimonials[current].company}
                </p>
              </div>
            </footer>
          </motion.blockquote>
        </AnimatePresence>

        {/* Dots navigation */}
        <div className="flex justify-center gap-2 mt-8">
          {testimonials.map((_, index) => (
            <button
              key={index}
              onClick={() => setCurrent(index)}
              className={cn(
                'w-2 h-2 rounded-full transition-all',
                index === current ? 'bg-primary w-6' : 'bg-muted'
              )}
              aria-label={`Go to testimonial ${index + 1}`}
            />
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## Build Tools & Workflows

### Vite Configuration (Recommended for 2026)

```typescript
// vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
  build: {
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['react', 'react-dom'],
          motion: ['framer-motion'],
          gsap: ['gsap'],
        },
      },
    },
  },
  // Enable CSS code splitting
  css: {
    modules: {
      localsConvention: 'camelCaseOnly',
    },
  },
});
```

### Modern Development Workflow

```json
// package.json scripts
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint && tsc --noEmit",
    "format": "prettier --write \"**/*.{ts,tsx,js,jsx,json,md}\"",
    "test": "vitest",
    "test:e2e": "playwright test",
    "storybook": "storybook dev -p 6006",
    "analyze": "ANALYZE=true next build"
  }
}
```

### CI/CD Pipeline

```yaml
# .github/workflows/ci.yml
name: CI

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v2
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'pnpm'

      - run: pnpm install
      - run: pnpm lint
      - run: pnpm test
      - run: pnpm build

  lighthouse:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - uses: actions/checkout@v4
      - name: Lighthouse CI
        uses: treosh/lighthouse-ci-action@v10
        with:
          configPath: ./lighthouserc.json
          uploadArtifacts: true
```

---

## Testing Strategy

### Unit Testing with Vitest

```typescript
// components/__tests__/button.test.tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { Button } from '../button';

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument();
  });

  it('calls onClick when clicked', async () => {
    const onClick = vi.fn();
    render(<Button onClick={onClick}>Click me</Button>);

    await userEvent.click(screen.getByRole('button'));
    expect(onClick).toHaveBeenCalledOnce();
  });

  it('shows loading state', () => {
    render(<Button loading>Submit</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
    expect(screen.getByTestId('loading-spinner')).toBeInTheDocument();
  });
});
```

### E2E Testing with Playwright

```typescript
// e2e/homepage.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Homepage', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/');
  });

  test('has correct title', async ({ page }) => {
    await expect(page).toHaveTitle(/Premium Agency/);
  });

  test('navigation works', async ({ page }) => {
    await page.click('text=Services');
    await expect(page).toHaveURL(/\/services/);
  });

  test('contact form submits', async ({ page }) => {
    await page.goto('/contact');
    await page.fill('[name="name"]', 'John Doe');
    await page.fill('[name="email"]', 'john@example.com');
    await page.fill('[name="message"]', 'Hello!');
    await page.click('button[type="submit"]');

    await expect(page.locator('.success-message')).toBeVisible();
  });
});
```

### Accessibility Testing

```typescript
// e2e/accessibility.spec.ts
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test.describe('Accessibility', () => {
  test('homepage has no violations', async ({ page }) => {
    await page.goto('/');

    const accessibilityScanResults = await new AxeBuilder({ page })
      .withTags(['wcag2a', 'wcag2aa'])
      .analyze();

    expect(accessibilityScanResults.violations).toEqual([]);
  });
});
```

---

## Integration Points

### With Web/UI/UX Design Skill

**Design Handoff Process:**
- Review Figma files for component patterns
- Extract design tokens (colors, typography, spacing)
- Identify responsive behavior expectations
- Clarify animation and interaction specifications
- Establish component naming conventions

**Collaboration Pattern:**
```
Design → Tokens → Components → Pages → Review → Polish
         ↑                              ↓
         └──────── Feedback ────────────┘
```

### With Strategic Technical SEO Skill

**Integration Points:**
- Semantic HTML structure for crawlability
- Meta tag implementation (title, description, OG, Twitter)
- Structured data (JSON-LD) implementation
- Image optimization with proper alt text
- Core Web Vitals optimization
- Internal linking structure
- XML sitemap generation

### With JavaScript SEO Skill

**Integration Points:**
- Rendering strategy selection (SSR/SSG/CSR)
- Client-side hydration optimization
- JavaScript bundle impact on crawling
- Dynamic content rendering verification
- URL structure for SPAs

### With Branding Expert Skill

**Integration Points:**
- Brand guideline implementation
- Typography system from brand standards
- Color palette with accessibility adjustments
- Logo usage across responsive breakpoints
- Micro-interactions reflecting brand personality

---

## Quality Standards

### Foundational Checklist

- [ ] Semantic HTML throughout
- [ ] All images have alt text
- [ ] Forms have proper labels
- [ ] Color contrast meets WCAG AA (4.5:1)
- [ ] Keyboard navigable
- [ ] Focus states visible
- [ ] Responsive from 320px to 1920px
- [ ] No horizontal scroll on mobile
- [ ] All links work
- [ ] Forms submit correctly
- [ ] Lighthouse Performance 75+

### Advanced Checklist

- [ ] TypeScript strict mode, no errors
- [ ] Component documentation (Storybook)
- [ ] Unit tests for utilities and hooks
- [ ] E2E tests for critical paths
- [ ] Lighthouse Performance 85+
- [ ] Accessibility automated testing passes
- [ ] Cross-browser testing (Chrome, Firefox, Safari, Edge)
- [ ] Print stylesheet (if applicable)
- [ ] Error boundaries for graceful failures
- [ ] Loading states for all async operations

### Premium Checklist

- [ ] Lighthouse 95+ (all metrics)
- [ ] WCAG 2.1 AA fully compliant (manual + automated)
- [ ] LCP < 1.5s on fast 3G
- [ ] INP < 100ms
- [ ] CLS < 0.05
- [ ] Animations respect `prefers-reduced-motion`
- [ ] Dark mode fully implemented
- [ ] Visual regression tests
- [ ] Performance budgets enforced in CI
- [ ] Security headers configured
- [ ] Zero console errors/warnings
- [ ] Real User Monitoring configured

---

## Pitfalls to Avoid

### Common Mistakes

1. **Not using semantic HTML**
   - ❌ `<div onclick="...">` for buttons
   - ✅ `<button onclick="...">` with proper type

2. **Ignoring accessibility until the end**
   - ❌ "We'll add accessibility later"
   - ✅ Accessibility as foundation, not afterthought

3. **Over-animating**
   - ❌ Everything moves, bounces, fades
   - ✅ Strategic, purposeful animations

4. **Not testing on real devices**
   - ❌ Only checking browser DevTools
   - ✅ Testing on actual phones, tablets

5. **Ignoring Core Web Vitals**
   - ❌ "Performance optimization at the end"
   - ✅ Performance budgets from the start

6. **Choosing framework based on hype**
   - ❌ "X is popular so we'll use it"
   - ✅ Choosing based on team expertise and project needs

### Premium-Specific Pitfalls

1. **Animations without reduced motion support**
   - ❌ Flashy animations that can't be disabled
   - ✅ All animations respect user preferences

2. **Premium visuals but poor performance**
   - ❌ Beautiful site that takes 8 seconds to load
   - ✅ Premium experience AND sub-2s loads

3. **Complex interactions without fallbacks**
   - ❌ 3D hero that breaks on older devices
   - ✅ Progressive enhancement for all features

4. **Overengineering for "scalability"**
   - ❌ Micro-frontend for 5-page marketing site
   - ✅ Right-sized architecture for actual needs

5. **Ignoring content editor experience**
   - ❌ Developer-friendly CMS that confuses clients
   - ✅ CMS tested with actual content team

---

## Key Principles

1. **Performance is UX.** Every millisecond matters. A fast site feels premium; a slow site feels cheap.

2. **Accessibility is non-negotiable.** It's not a feature—it's a requirement. Build it in from day one.

3. **Mobile-first is the only way.** Start with constraints, enhance with space.

4. **Semantic HTML is your foundation.** Divs with classes are not structure. Use the right elements.

5. **CSS custom properties enable scale.** Design tokens create consistency and make theming possible.

6. **Animation should have purpose.** Every motion should communicate something—guide attention, provide feedback, create delight.

7. **TypeScript prevents bugs.** The overhead is worth it. Strict mode catches errors before users do.

8. **Test what matters.** Unit tests for logic, E2E for critical paths, visual regression for design systems.

9. **Choose boring technology.** Proven frameworks over bleeding edge. Your client doesn't need to beta test.

10. **Premium is in the details.** The difference between good and exceptional is 100 small things done right.

---

## Resources

### Development Tools

**Build & Bundling:**
- Vite - Modern build tool
- Next.js - React framework with SSR/SSG
- Nuxt - Vue framework with SSR/SSG
- Astro - Content-focused static sites

**Animation:**
- GSAP - Professional animation library
- Framer Motion - React animation library
- Lenis - Smooth scrolling
- Three.js / React Three Fiber - 3D/WebGL

**CMS:**
- Sanity - Real-time headless CMS
- Contentful - Enterprise headless CMS
- Strapi - Self-hosted headless CMS
- Prismic - Visual editing headless CMS

**Testing:**
- Vitest - Unit testing
- Playwright - E2E testing
- Storybook - Component documentation
- Axe - Accessibility testing

### Learning Resources

**Documentation:**
- MDN Web Docs - Web standards reference
- web.dev - Google's web development guidance
- React/Vue/Next/Nuxt official docs

**Performance:**
- web.dev/vitals - Core Web Vitals guide
- WebPageTest - Performance testing
- Lighthouse - Automated auditing

**Accessibility:**
- WebAIM - Accessibility resources
- A11y Project - Accessibility patterns
- Inclusive Components - Accessible component patterns

### Reference Materials

**Design Systems:**
- Radix UI - Unstyled accessible components
- shadcn/ui - Re-usable component library
- Tailwind UI - Premium Tailwind components

**Inspiration:**
- Awwwards - Design inspiration
- Codrops - Creative tutorials
- CSS-Tricks - CSS techniques

---

*This skill enables premium frontend development that transforms design visions into exceptional digital experiences—the kind of work that justifies $15-50k+ website builds and positions Haide Digital as a leader in boutique agency execution.*
