---
name: haide-brand-guidelines
description: Applies Haide Digital's official brand identity, colors, typography, and visual language to any artifact. Use when generating websites, presentations, documents, or any visual output that should reflect the Haide Digital brand.
trigger_terms_primary:
  - brand colors
  - brand guidelines
  - haide styling
  - design system
  - visual identity
  - brand assets
trigger_terms_contextual:
  - make it look like haide
  - apply our brand
  - use our colors
  - style this
  - format this professionally
trigger_terms_client_language:
  - make it on-brand
  - use company colors
  - corporate styling
related_skills:
  frequently_used_with:
    - frontend-design
    - ui-ux-pro-max
license: Complete terms in LICENSE.txt
---

# Haide Digital Brand Guidelines

## Overview

This skill applies Haide Digital's official brand identity to any visual artifact — websites, presentations, documents, UI components, or marketing materials.

**Keywords**: branding, Haide Digital, visual identity, brand colors, typography, design system, corporate identity, styling, visual formatting

---

## 🎯 Design Philosophy

**Brand Positioning:** Systems for organic growth — not traditional consultancy.

**Aesthetic Direction:** Premium Technical Authority meets Approachable Expertise.

We blend the corporate trust of enterprise SEO agencies with the fresh, modern energy of boutique digital studios. The result is sophisticated but not stuffy, technical but not intimidating.

**Design Principles:**
1. **Confident, not cocky** — Let results speak. No hype language.
2. **Technical credibility** — Show expertise through precision, not decoration.
3. **Bulgarian cost advantage, enterprise quality** — Premium execution at accessible pricing.
4. **Systems-oriented** — Visual language reinforces systematic, predictable approaches.

---

## 🎨 Color System

### Primary Colors

| Name | Hex | RGB | Usage |
|------|-----|-----|-------|
| **Haide Orange** | `#FF5B04` | rgb(255, 91, 4) | Primary accent, CTAs, highlights, active states |
| Haide Orange Light | `#FF7D3A` | rgb(255, 125, 58) | Hover states, secondary accents |
| Haide Orange Dark | `#E04F00` | rgb(224, 79, 0) | Pressed states, dark mode accent |

### Secondary Colors

| Name | Hex | RGB | Usage |
|------|-----|-----|-------|
| **Deep Sea Green** | `#075056` | rgb(7, 80, 86) | Secondary accent, trust elements, headers |
| Deep Sea Light | `#0A6B73` | rgb(10, 107, 115) | Hover states |
| Deep Sea Dark | `#053B40` | rgb(5, 59, 64) | Dark backgrounds, footers |

### Tertiary & Background Colors

| Name | Hex | RGB | Usage |
|------|-----|-----|-------|
| **Mirage** | `#16232A` | rgb(22, 35, 42) | Primary text, dark backgrounds |
| Mirage Light | `#1E3340` | rgb(30, 51, 64) | Card backgrounds (dark mode) |
| **Wild Sand** | `#E4EEF0` | rgb(228, 238, 240) | Light backgrounds, sections |
| Wild Sand Dark | `#C8DDE1` | rgb(200, 221, 225) | Borders, dividers |

### Supporting Colors

| Name | Hex | Usage |
|------|-----|-------|
| **Gunmetal** | `#233038` | Alternative dark text, dark UI elements |
| Gunmetal Light | `#2E3F49` | Secondary dark backgrounds |
| **Ivory Cream** | `#FDF6E3` | Warm light backgrounds, callout boxes |
| Ivory Dark | `#F5EBCF` | Warm borders |
| **Sand Yellow** | `#F4D47C` | Highlights, awards, premium badges |
| Sand Yellow Light | `#F7DF9E` | Subtle highlights |
| **Light Silver** | `#D3DBDD` | Subtle borders, disabled states |

### Semantic Colors

| Purpose | Hex | Usage |
|---------|-----|-------|
| Success | `#22C55E` | Success messages, positive metrics |
| Warning | `#F59E0B` | Warnings, caution states |
| Error | `#EF4444` | Errors, destructive actions |
| Info | `#3B82F6` | Informational elements |

### Text Colors

| Name | Hex | Usage |
|------|-----|-------|
| Text Primary | `#16232A` | Main body text, headlines |
| Text Secondary | `#6B7280` | Captions, labels, helper text |
| Text Inverse | `#FFFFFF` | Text on dark backgrounds |

### CSS Variables
```css
:root {
  /* Primary — Haide Orange */
  --color-primary: #FF5B04;
  --color-primary-light: #FF7D3A;
  --color-primary-dark: #E04F00;

  /* Secondary — Deep Sea Green */
  --color-secondary: #075056;
  --color-secondary-light: #0A6B73;
  --color-secondary-dark: #053B40;

  /* Tertiary — Mirage */
  --color-tertiary: #16232A;
  --color-tertiary-light: #1E3340;

  /* Base — Wild Sand */
  --color-base: #E4EEF0;
  --color-base-dark: #C8DDE1;

  /* Supporting */
  --color-gunmetal: #233038;
  --color-ivory: #FDF6E3;
  --color-sand-yellow: #F4D47C;
  --color-silver: #D3DBDD;

  /* Text */
  --color-text-primary: #16232A;
  --color-text-secondary: #6B7280;
  --color-text-inverse: #FFFFFF;

  /* Semantic */
  --color-success: #22C55E;
  --color-warning: #F59E0B;
  --color-error: #EF4444;
  --color-info: #3B82F6;
}
```

### Tailwind Config
```javascript
colors: {
  haide: {
    orange: {
      DEFAULT: '#FF5B04',
      light: '#FF7D3A',
      dark: '#E04F00',
    },
    sea: {
      DEFAULT: '#075056',
      light: '#0A6B73',
      dark: '#053B40',
    },
    mirage: {
      DEFAULT: '#16232A',
      light: '#1E3340',
    },
    sand: {
      DEFAULT: '#E4EEF0',
      dark: '#C8DDE1',
    },
    gunmetal: {
      DEFAULT: '#233038',
      light: '#2E3F49',
      dark: '#1A252B',
    },
    ivory: {
      DEFAULT: '#FDF6E3',
      dark: '#F5EBCF',
    },
    'sand-yellow': {
      DEFAULT: '#F4D47C',
      light: '#F7DF9E',
      dark: '#E8C25A',
    },
    silver: {
      DEFAULT: '#D3DBDD',
      dark: '#B8C4C7',
    },
  },
}
```

---

## ✍️ Typography

### Font Stack

| Purpose | Font | Fallback | Usage |
|---------|------|----------|-------|
| **Headlines** | Poppins | system-ui, sans-serif | H1-H6, display text, nav, buttons |
| **Body** | Noto Sans | Inter, system-ui, sans-serif | Paragraphs, UI text, inputs |
| **Monospace** | JetBrains Mono | Fira Code, monospace | Code, metrics, technical data |

### Google Fonts Import
```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=JetBrains+Mono:wght@400;500&family=Noto+Sans:ital,wght@0,400;0,500;0,600;1,400;1,600&family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Typography Patterns

**Headlines (Poppins):**
- H1: 48-72px, SemiBold 600, tracking -0.02em
- H2: 36-48px, SemiBold 600, tracking -0.01em
- H3: 28-36px, Medium 500
- H4: 24px, Medium 500
- H5: 20px, Medium 500
- H6: 18px, Medium 500

**Body Text (Noto Sans):**
- Body Large: 18px, Regular 400, line-height 1.6
- Body: 16px, Regular 400, line-height 1.5
- Body Small: 14px, Regular 400, line-height 1.5
- Caption: 12px, Regular 400

**Component Typography:**

| Element | Font | Size | Weight | Notes |
|---------|------|------|--------|-------|
| Section Eyebrow | Poppins | 12px | SemiBold 600 | Uppercase, tracking 0.08em, Haide Orange |
| Nav Links | Poppins | 16px | Medium 500 | Active: SemiBold + Orange |
| Buttons | Poppins | 14-16px | Medium 500 | Uppercase optional for primary |
| Badges/Tags | Poppins | 12px | Medium 500 | Uppercase, tracking 0.05em |
| Form Labels | Poppins | 14px | Medium 500 | — |
| Form Inputs | Noto Sans | 16px | Regular 400 | — |
| Stat Numbers | JetBrains Mono | 48-72px | Medium 500 | Haide Orange |
| Stat Labels | Noto Sans | 14px | Medium 500 | Text Secondary |
| Testimonial Quote | Noto Sans | 18-20px | SemiBold Italic 600i | — |
| Attribution | Noto Sans | 14px | Medium 500 | — |
| Callout Heading | Poppins | 16-18px | Medium 500 | — |
| Callout Body | Noto Sans | 14-16px | Medium 500 | — |

### Typography Rules

**DO:**
- Use Poppins for all headlines and navigation
- Use Noto Sans for all body text and UI elements
- Use JetBrains Mono for code and metrics
- Use SemiBold (600) for emphasis in body text
- Use italic for testimonials and editorial asides

**NEVER:**
- Never use Poppins for body paragraphs
- Never use Noto Sans for headlines (H1-H5)
- Never italicize Poppins headlines
- Never use more than one bold phrase per sentence in body text
- Never combine italic + bold in body text (except pull quotes)

### CSS Typography Variables
```css
:root {
  /* Font Families */
  --font-display: 'Poppins', system-ui, sans-serif;
  --font-body: 'Noto Sans', 'Inter', system-ui, sans-serif;
  --font-mono: 'JetBrains Mono', 'Fira Code', monospace;

  /* Font Sizes */
  --text-xs: 0.75rem;    /* 12px */
  --text-sm: 0.875rem;   /* 14px */
  --text-base: 1rem;     /* 16px */
  --text-lg: 1.125rem;   /* 18px */
  --text-xl: 1.25rem;    /* 20px */
  --text-2xl: 1.5rem;    /* 24px */
  --text-3xl: 1.75rem;   /* 28px */
  --text-4xl: 2.25rem;   /* 36px */
  --text-5xl: 3rem;      /* 48px */
  --text-6xl: 3.5rem;    /* 56px */
  --text-7xl: 4.5rem;    /* 72px */

  /* Font Weights */
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;

  /* Letter Spacing */
  --tracking-tight: -0.02em;
  --tracking-snug: -0.01em;
  --tracking-normal: 0;
  --tracking-wide: 0.01em;
  --tracking-wider: 0.05em;
  --tracking-widest: 0.08em;
}
```

---

## 📐 Spacing & Layout

### 8-Point Grid System

All spacing uses multiples of 8px:

| Token | Value | Usage |
|-------|-------|-------|
| xs | 4px | Tight gaps, icon padding |
| sm | 8px | Compact spacing |
| md | 16px | Standard component padding |
| lg | 24px | Section gaps |
| xl | 32px | Large gaps |
| 2xl | 48px | Section padding |
| 3xl | 64px | Major section breaks |
| 4xl | 96px | Hero/feature spacing |

### Container Widths

```css
:root {
  --container-sm: 640px;
  --container-md: 768px;
  --container-lg: 1024px;
  --container-xl: 1280px;
  --container-2xl: 1440px;
}
```

### Responsive Breakpoints

```css
/* Mobile-first breakpoints */
--bp-sm: 480px;   /* Large phones */
--bp-md: 768px;   /* Tablets */
--bp-lg: 1024px;  /* Small laptops */
--bp-xl: 1280px;  /* Desktop */
--bp-2xl: 1440px; /* Large desktop */
```

---

## 🎭 Visual Effects

### Shadows

```css
:root {
  /* Standard shadows */
  --shadow-sm: 0 1px 2px 0 rgb(0 0 0 / 0.05);
  --shadow-md: 0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1);
  --shadow-lg: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
  --shadow-xl: 0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1);
  --shadow-2xl: 0 25px 50px -12px rgb(0 0 0 / 0.25);

  /* Colored shadows for cards */
  --shadow-orange: 0 10px 40px -10px rgb(255 91 4 / 0.3);
  --shadow-sea: 0 10px 40px -10px rgb(7 80 86 / 0.2);
  --shadow-mirage: 0 10px 40px -10px rgb(22 35 42 / 0.15);
  --shadow-gunmetal: 0 10px 40px -10px rgb(35 48 56 / 0.2);
  --shadow-yellow: 0 10px 40px -10px rgb(244 212 124 / 0.3);
}
```

### Border Radius

| Token | Value | Usage |
|-------|-------|-------|
| none | 0 | Sharp corners |
| sm | 4px | Subtle rounding |
| md | 8px | Standard components |
| lg | 12px | Cards, modals |
| xl | 16px | Large cards |
| 2xl | 24px | Feature cards |
| full | 9999px | Pills, avatars |

---

## 🧩 Component Patterns

### Buttons

**Primary Button:**
- Background: Haide Orange `#FF5B04`
- Text: White, Poppins Medium 500
- Hover: Orange Light `#FF7D3A`
- Shadow: `--shadow-orange` on hover

**Secondary Button:**
- Background: Deep Sea Green `#075056`
- Text: White, Poppins Medium 500
- Hover: Deep Sea Light `#0A6B73`
- Shadow: `--shadow-sea` on hover

**Ghost Button:**
- Background: Transparent
- Border: 2px solid Haide Orange
- Text: Haide Orange, Poppins Medium 500
- Hover: Orange background, white text

### Cards

**Standard Card:**
- Background: White
- Border: 1px solid `--color-silver`
- Border-radius: 12px
- Padding: 24px
- Shadow: `--shadow-md`
- Hover: `--shadow-lg`

**Feature Card:**
- Background: Wild Sand `#E4EEF0`
- Border-left: 4px solid Haide Orange
- Border-radius: 8px
- Padding: 24px 32px

### Callout Box

- Background: Ivory Cream `#FDF6E3`
- Border-left: 3px solid Deep Sea Green
- Border-radius: 0 8px 8px 0
- Padding: 16px 24px

---

## ✅ Brand Application Checklist

When applying Haide Digital branding:

- [ ] Primary accent color is Haide Orange `#FF5B04`
- [ ] Headlines use Poppins font
- [ ] Body text uses Noto Sans font
- [ ] Metrics/code use JetBrains Mono
- [ ] 8-point grid spacing applied
- [ ] CTAs use orange or deep sea green
- [ ] Dark backgrounds use Mirage `#16232A` or Gunmetal `#233038`
- [ ] Light backgrounds use Wild Sand `#E4EEF0` or Ivory `#FDF6E3`
- [ ] No italic on Poppins headlines
- [ ] No Poppins in body paragraphs

---

## 🚫 Common Mistakes

| Mistake | Correction |
|---------|------------|
| Using Poppins for paragraphs | Use Noto Sans for all body text |
| Using a different orange | Always use `#FF5B04` for brand orange |
| Pure black text | Use Mirage `#16232A` for dark text |
| Pure white backgrounds | Use Off-white `#F5F9FA` or Wild Sand `#E4EEF0` |
| Italicizing headlines | Never italicize Poppins |
| Generic blue CTAs | Use Haide Orange or Deep Sea Green |
| Inconsistent spacing | Follow 8-point grid (8, 16, 24, 32, 48, 64px) |

---

## Resources

- **Full Design System:** [Brand Reference/design_system.md](../../Brand%20Reference/design_system.md)
- **Brand Positioning:** [Brand Reference/Brand_Positioning.md](../../Brand%20Reference/Brand_Positioning.md)
- **Core Values:** [Brand Reference/Core_Values_Manifesto.md](../../Brand%20Reference/Core_Values_Manifesto.md)
