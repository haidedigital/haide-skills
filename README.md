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
| [brand-guidelines](brand-and-design/brand-guidelines/) | Brand guidelines enforcement, visual identity standards |
| [frontend-design](brand-and-design/frontend-design/) | Frontend design patterns, layout systems, responsive design |

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

### Testing
| Skill | Description |
|---|---|
| [testing](testing/testing/) | Vitest unit/integration, Playwright E2E, visual regression, MSW mocking, CI test pipeline |

### Performance & Quality
| Skill | Description |
|---|---|
| [performance-budget](performance-and-quality/performance-budget/) | CWV enforcement, asset budgets, Lighthouse 95+ target |
| [mobile-first-haide](performance-and-quality/mobile-first-haide/) | Touch targets, responsive animations, mobile audit checklist |
| [component-audit](performance-and-quality/component-audit/) | 10-domain pre-ship gate (Brand, Arch, Animation, Mobile, Perf, Copy, SEO, Security, A11y, Error Handling) |

### Design & Video Production
| Skill | Description |
|---|---|
| [stitch](design/stitch/) | Stitch design-to-code suite — 6 sub-skills: design-md, enhance-prompt, react-components, remotion, shadcn-ui, stitch-loop |

### Automation (n8n)
| Skill | Description |
|---|---|
| [n8n-mcp-tools-expert](automation/n8n-mcp-tools-expert/) | n8n MCP tools — search, validation, workflow guides |
| [n8n-workflow-patterns](automation/n8n-workflow-patterns/) | n8n workflow architecture and design patterns |
| [n8n-expression-syntax](automation/n8n-expression-syntax/) | n8n expression language reference |
| [n8n-validation-expert](automation/n8n-validation-expert/) | n8n data validation and error handling |
| [n8n-node-configuration](automation/n8n-node-configuration/) | n8n node setup and configuration |
| [n8n-code-javascript](automation/n8n-code-javascript/) | JavaScript code nodes in n8n |
| [n8n-code-python](automation/n8n-code-python/) | Python code nodes in n8n |

### Research
| Skill | Description |
|---|---|
| [notebooklm-research](research/notebooklm-research/) | NotebookLM-powered deep research workflows |

### Process & Documentation
| Skill | Description |
|---|---|
| [action-catalog](process/action-catalog/) | Documents build actions for case studies, YouTube, and SOPs |
| [skill-registry](process/skill-registry/) | Post-creation pipeline — package, push to GitHub, update CLAUDE.md, sync Notion, audit codebase |
| [skill-creator](process/skill-creator/) | Skill creation toolkit — init, validate, package scripts |

### Knowledge Base (Reference Files)
| File | Domain | Size |
|---|---|---|
| [Automation_Systems_Design](knowledge-base/automation-systems-design/) | Boutique efficiency, automation frameworks | 44KB |
| [Branding_Expert](knowledge-base/branding-expert/) | Category leadership, premium positioning | 42KB |
| [Business_Mastery](knowledge-base/business-mastery/) | Boutique scaling, A-player hiring, exit readiness | 26KB |
| [Client_Communication](knowledge-base/client-communication/) | Strategic advisory, C-suite communication | 36KB |
| [Commercial_Judgment](knowledge-base/commercial-judgment/) | Premium pricing psychology, LTV optimization | 44KB |
| [Content_Strategy_IA](knowledge-base/content-strategy/) | Topical authority, pillar-cluster architecture | 69KB |
| [Data_Measurement](knowledge-base/data-measurement/) | Performance intelligence, executive reporting | 69KB |
| [Editorial_Copywriting](knowledge-base/copywriting/) | Premium copywriting, thought leadership | 49KB |
| [Frontend_Development](knowledge-base/frontend-development/) | HTML/CSS/JS patterns, frameworks | 91KB |
| [GEO_SGO](knowledge-base/geo-sgo/) | AI search optimization | 38KB |
| [Javascript_SEO](knowledge-base/javascript-seo/) | JS rendering, crawlability, CWV | 37KB |
| [Link_Strategy_Authority](knowledge-base/link-strategy-authority/) | Earned media, digital PR, authority building | 51KB |
| [Maintenance_Optimization_Mindset](knowledge-base/maintenance-optimization-mindset/) | Continuous improvement, optimization cycles | 36KB |
| [Project_Management](knowledge-base/project-management/) | Client success orchestration, delivery excellence | 30KB |
| [Project_Process_Ownership](knowledge-base/project-process-ownership/) | Scope control, sequencing, dependencies | 33KB |
| [QA_SEO_Risk_Management](knowledge-base/qa-seo-risk-management/) | Premium QA, risk mitigation, crisis protocols | 64KB |
| [Social_Distribution_Strategy](knowledge-base/social-distribution-strategy/) | Brand amplification, executive thought leadership | 46KB |
| [Strategic_Technical_SEO](knowledge-base/strategic-technical-seo/) | Enterprise technical infrastructure, migrations | 32KB |
| [Thought_Leadership](knowledge-base/thought-leadership/) | Authority building, executive personal branding | 64KB |
| [Tooling_API_Literacy](knowledge-base/tooling-api-literacy/) | Best-in-class tooling, API integration | 44KB |
| [UI_UX_Pro_Max](knowledge-base/ui-ux/) | Design systems, 67 UI styles, palettes | 52KB |
| [Visual_Content_Direction](knowledge-base/visual-content/) | Creative briefing, brand visuals | 48KB |
| [Web_UI_UX_Design](knowledge-base/web-ui-ux-design/) | Premium digital experiences, UX frameworks | 47KB |

## How To Use

1. Clone this repo: `git clone https://github.com/haidedigital/haide-skills.git`
2. Copy the skill directory (or `.skill` ZIP) into your project root
3. Reference it in your project's `CLAUDE.md`
4. Claude Code will auto-detect and load skills when relevant

## Master Index

The [skills.md](skills.md) file contains the full agency skills index with scenario-based skill selection, premium context flags, and AI agent activation guides.

## Projects Using These Skills

- **Haide Digital Website** (haide.digital) — Next.js 14, all skills active
- **Social Media OS** — Content operations, n8n automation
- **Bondex Content Automations** — n8n workflow skills
- **NotebookLM Research** — Deep research workflows
