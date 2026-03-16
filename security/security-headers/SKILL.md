---
name: security-headers
description: >-
  Security standards for the Haide Digital website — a Next.js 14 marketing/lead-gen site
  on Cloudflare Pages. Covers Content-Security-Policy, security response headers, contact
  form hardening (rate limiting, honeypot, CSRF, input sanitization), environment variable
  hygiene, dependency auditing, PII handling, cookie consent (EU/Bulgaria), and third-party
  script governance. Use when adding API routes, configuring headers, handling form
  submissions, adding analytics/tracking scripts, reviewing security posture, or deploying
  to production. Triggers on: security, CSP, headers, CSRF, rate limit, honeypot, cookie
  consent, GDPR, PII, env variables, API security, input sanitization, XSS,
  Content-Security-Policy, permissions policy, third-party scripts, analytics consent.
---

# Security Headers — Haide Digital

## Architecture Context

- **Framework:** Next.js 14 App Router
- **Hosting:** Cloudflare Pages
- **Site type:** Client-facing marketing + lead generation
- **Region:** EU (Bulgaria) — GDPR applies

---

## Security Headers (next.config.ts)

Define in `headers()` — applied to every response:

```ts
// next.config.ts
const securityHeaders = [
  {
    key: "Content-Security-Policy",
    value: [
      "default-src 'self'",
      "script-src 'self' 'unsafe-inline' https://www.googletagmanager.com https://www.google-analytics.com",
      "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
      "font-src 'self' https://fonts.gstatic.com",
      "img-src 'self' data: blob:",
      "connect-src 'self' https://www.google-analytics.com https://analytics.google.com",
      "frame-ancestors 'none'",
    ].join("; "),
  },
  { key: "X-Frame-Options", value: "DENY" },
  { key: "X-Content-Type-Options", value: "nosniff" },
  { key: "Referrer-Policy", value: "strict-origin-when-cross-origin" },
  { key: "Permissions-Policy", value: "camera=(), microphone=(), geolocation=()" },
  { key: "Strict-Transport-Security", value: "max-age=31536000; includeSubDomains" },
];

module.exports = {
  async headers() {
    return [{ source: "/(.*)", headers: securityHeaders }];
  },
};
```

**Rules:**
- `'unsafe-inline'` for scripts — required by Next.js inline scripts. Do NOT add `'unsafe-eval'`
- `'unsafe-inline'` for styles — required by Tailwind runtime
- `blob:` in img-src — required for Three.js canvas exports
- `frame-ancestors 'none'` — prevents clickjacking (mirrors X-Frame-Options)
- Verify headers pass [securityheaders.com](https://securityheaders.com) after deployment
- Check browser console for CSP errors — add legitimate sources, never use wildcard `*`

---

## Contact Form Security

See `references/form-security.md` for full implementation patterns.

**Mandatory checklist for any form:**
1. **Input sanitization** — strip HTML tags from all text inputs before processing
2. **Rate limiting** — max 3 submissions per IP per hour
3. **Honeypot field** — invisible input that bots fill; reject silently if populated
4. **CSRF token** — required on every state-changing form
5. **Server-side validation** — duplicate frontend Zod schema on API route
6. **Route-only submission** — forms POST to `/api/contact`, never directly to third-party

---

## Environment Variables

```
.env.local          # Local dev secrets (NEVER committed)
.env.example         # Template with dummy values (committed)
```

**Rules:**
- All secrets in `.env.local` — never in `.env` (which may get committed)
- `NEXT_PUBLIC_` prefix only for values that genuinely need client exposure
- API keys for Sanity, email services, analytics — server-side only (no `NEXT_PUBLIC_`)
- `.gitignore` must include: `.env*`, `.env.local`, `.env.production`
- Never hardcode secrets in source files — always `process.env.SECRET_NAME`

---

## PII Handling

- Never `console.log` email addresses, phone numbers, or names
- Never store PII in `localStorage` or `sessionStorage`
- GA4: enable privacy mode (no IP tracking, anonymized data)
- Hash or omit PII in server logs — log only timestamps and inquiry type
- Contact form data: persist to database or email service, never to logs

---

## Cookie Consent (GDPR — mandatory)

Bulgaria is EU — cookie consent is legally required before any tracking.

**Rules:**
- Cookie banner must appear before any tracking scripts fire
- No GA4, GTM, Hotjar, or session recording until explicit consent
- Store consent state in a first-party cookie (not localStorage)
- Provide granular choices: Analytics / Marketing / Functional
- "Reject all" must be equally prominent as "Accept all"

---

## Third-Party Scripts

| Script | Load Strategy | Consent Required |
|---|---|---|
| Google Tag Manager | `strategy="lazyOnload"` via `next/script` | Yes — after consent |
| Google Analytics (GA4) | Fire only after cookie consent granted | Yes |
| Chat widget | Load on user interaction (click/scroll), not page load | Yes |
| Hotjar / session recording | Only after explicit consent | Yes |

**Never** load tracking scripts in `<head>` or on initial render.

---

## API Route Security (`/app/api/**`)

- Validate `Content-Type: application/json` header on POST routes
- Reject unexpected HTTP methods (return 405)
- Never return stack traces or internal error details to client
- Return generic error messages externally; log details server-side (without PII)
- Validate request origin against allowed domains

---

## Dependency Security

- Run `pnpm audit` (or `npm audit`) before every deployment
- No packages with known critical vulnerabilities may ship
- Lock file (`pnpm-lock.yaml`) always committed
- Before adding a new dependency: check npm download count, last publish date, maintainer count
- Prefer well-maintained packages with >10K weekly downloads

---

## Verification Checklist

Run before every production deployment:

- [ ] Security headers present (verify at securityheaders.com)
- [ ] No API keys in client-side bundle (`pnpm build` then search output)
- [ ] Contact form has rate limiting and honeypot
- [ ] `.env` files not in git history (`git log --all -- .env*`)
- [ ] `pnpm audit` passes with no critical vulnerabilities
- [ ] Cookie consent fires before any analytics
- [ ] CSP not blocking legitimate resources (check console)

---

## References

- `references/form-security.md` — Full contact form security patterns: rate limiting, honeypot, CSRF, input sanitization, server-side Zod validation
- `references/csp-guide.md` — CSP directive reference, common Next.js/Cloudflare gotchas, testing patterns, nonce-based script loading
