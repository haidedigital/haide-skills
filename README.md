# Haide Skills Library

Organized collection of Claude Code skills used across Haide Digital / Antigravity projects.

## What Are Skills?

Skills are domain-specific instruction sets that Claude Code loads to enforce patterns, standards, and conventions. Each skill contains:

- `SKILL.md` — The main instruction file
- `references/` — Supporting docs (patterns, checklists, templates)

Skills override Claude's general behavior with project-specific rules.

## Categories

### Brand & Design
| Skill | Description |
|---|---|
| [haide-design-system](brand-and-design/haide-design-system/) | Brand colors, typography, buttons, section patterns, visual hierarchy |
| [haide-copy-voice](brand-and-design/haide-copy-voice/) | Brand voice, forbidden words, CTA patterns, microcopy, tone |

### Architecture & Code
| Skill | Description |
|---|---|
| [haide-nextjs-architecture](architecture/haide-nextjs-architecture/) | File structure, server/client boundaries, TypeScript, dynamic imports |
| [loading-screens](architecture/loading-screens/) | Page loader state machine, FOUC prevention, GSAP page transitions |

### Animation & 3D
| Skill | Description |
|---|---|
| [gsap-haide-animations](animation/gsap-haide-animations/) | GSAP 3 motion system — scroll reveals, text animations, Lenis |
| [three-js-components](animation/three-js-components/) | React Three Fiber — 3D heroes, particles, shaders, SSR-safe |

### SEO
| Skill | Description |
|---|---|
| [haide-seo](seo/haide-seo/) | Structured data, meta tags, GEO optimization, technical SEO |

### Security
| Skill | Description |
|---|---|
| [security-headers](security/security-headers/) | CSP, security headers, form hardening, GDPR cookie consent, PII handling |

### Analytics
| Skill | Description |
|---|---|
| [analytics-tracking](analytics/analytics-tracking/) | GTM/GA4 integration, event taxonomy, cookie consent, UTM handling, Search Console |

### Accessibility
| Skill | Description |
|---|---|
| [accessibility-a11y](accessibility/accessibility-a11y/) | WCAG 2.1 AA compliance, contrast ratios, ARIA patterns, keyboard nav, focus management |

### Error Handling & Monitoring
| Skill | Description |
|---|---|
| [error-handling-monitoring](error-handling/error-handling-monitoring/) | Error boundaries, Sentry, fallback UI, API error contracts, ChunkLoadError recovery |

### CI/CD & Deployment
| Skill | Description |
|---|---|
| [cicd-deployment](cicd/cicd-deployment/) | GitHub Actions pipeline, Vercel config, Lighthouse CI, bundle analysis, rollback |

### Performance & Quality
| Skill | Description |
|---|---|
| [performance-budget](performance-and-quality/performance-budget/) | CWV enforcement, asset budgets, Lighthouse 95+ target |
| [mobile-first-haide](performance-and-quality/mobile-first-haide/) | Touch targets, responsive animations, mobile audit checklist |
| [component-audit](performance-and-quality/component-audit/) | 10-domain pre-ship gate (Brand, Arch, Animation, Mobile, Perf, Copy, SEO, Security, A11y, Error Handling) |

### Process & Documentation
| Skill | Description |
|---|---|
| [action-catalog](process/action-catalog/) | Documents build actions for case studies, YouTube, and SOPs |

### Knowledge Base (Reference Files)
| File | Domain | Size |
|---|---|---|
| [Content_Strategy_IA](knowledge-base/content-strategy/) | Topical authority, pillar-cluster architecture | 69KB |
| [Editorial_Copywriting](knowledge-base/copywriting/) | Premium copywriting, thought leadership | 49KB |
| [Frontend_Development](knowledge-base/frontend-development/) | HTML/CSS/JS patterns, frameworks | 91KB |
| [GEO_SGO](knowledge-base/geo-sgo/) | AI search optimization | 38KB |
| [Javascript_SEO](knowledge-base/javascript-seo/) | JS rendering, crawlability, CWV | 37KB |
| [UI_UX_Pro_Max](knowledge-base/ui-ux/) | Design systems, 67 UI styles, palettes | 52KB |
| [Visual_Content_Direction](knowledge-base/visual-content/) | Creative briefing, brand visuals | 48KB |

## How To Use

1. Clone this repo: `git clone https://github.com/haidedigital/haide-skills.git`
2. Copy the skill directory (or `.skill` ZIP) into your project root
3. Reference it in your project's `CLAUDE.md`
4. Claude Code will auto-detect and load skills when relevant

## Projects Using These Skills

- **Haide Digital Website** (haide.digital) — Next.js 14, all skills active
