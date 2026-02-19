# Content-Security-Policy Guide

## Table of Contents
1. CSP Directives Reference
2. Next.js Specific Considerations
3. Cloudflare Pages Considerations
4. Testing CSP
5. Common Issues and Fixes
6. Nonce-Based Script Loading

---

## 1. CSP Directives Reference

| Directive | Purpose | Haide Value |
|---|---|---|
| `default-src` | Fallback for all resource types | `'self'` |
| `script-src` | JavaScript sources | `'self' 'unsafe-inline'` + GTM, GA |
| `style-src` | CSS sources | `'self' 'unsafe-inline'` + Google Fonts |
| `font-src` | Font file sources | `'self' fonts.gstatic.com` |
| `img-src` | Image sources | `'self' data: blob:` |
| `connect-src` | XHR, fetch, WebSocket targets | `'self'` + analytics endpoints |
| `frame-ancestors` | Who can embed this page | `'none'` |
| `media-src` | Audio/video sources | Not set (falls back to `default-src`) |
| `object-src` | Plugins (Flash, Java) | Not set (falls back to `default-src 'self'`) |
| `base-uri` | Restricts `<base>` tag | Not set (consider adding `'self'`) |
| `form-action` | Form submission targets | Not set (consider adding `'self'`) |

---

## 2. Next.js Specific Considerations

**Why `'unsafe-inline'` is required for scripts:**
Next.js injects inline `<script>` tags for hydration data (`__NEXT_DATA__`), route prefetching, and chunk loading. Removing `'unsafe-inline'` from `script-src` breaks hydration.

**Future improvement — nonce-based CSP:**
Next.js 14+ supports nonce-based CSP via middleware. This is more secure than `'unsafe-inline'` but requires middleware configuration. See Section 6 for implementation.

**Why `'unsafe-inline'` is required for styles:**
Tailwind CSS and styled-jsx inject inline styles. Removing `'unsafe-inline'` from `style-src` breaks styling.

**Dynamic imports:**
Next.js `dynamic()` imports load chunks from the same origin. No additional CSP source needed as long as `'self'` is in `script-src`.

**`next/image`:**
Remote images require adding the hostname to `img-src`. For local images only, `'self' data: blob:` is sufficient.

**`next/script`:**
Scripts loaded with `next/script` respect CSP. Use `strategy="lazyOnload"` for third-party scripts to defer loading.

---

## 3. Cloudflare Pages Considerations

**Headers via `_headers` file:**
Cloudflare Pages also supports a `_headers` file in the `public/` directory. However, prefer `next.config.ts` `headers()` for consistency with the Next.js build pipeline.

```
# public/_headers (alternative approach)
/*
  X-Frame-Options: DENY
  X-Content-Type-Options: nosniff
  Referrer-Policy: strict-origin-when-cross-origin
  Permissions-Policy: camera=(), microphone=(), geolocation=()
```

**Cloudflare automatic headers:**
- Cloudflare adds `Strict-Transport-Security` automatically for zones with "Always Use HTTPS" enabled
- Verify with `curl -I https://haide.digital` that HSTS is present
- If not, add it explicitly in `next.config.ts`

**Cloudflare caching and CSP:**
CSP headers are not cached by Cloudflare CDN by default. They pass through from the origin. No special handling needed.

---

## 4. Testing CSP

**Browser DevTools:**
1. Open DevTools > Console
2. Look for `Refused to load` or `Blocked by Content Security Policy` errors
3. Each error tells you exactly which directive is blocking and what source was attempted

**securityheaders.com:**
1. Enter `https://haide.digital`
2. Check for A+ rating
3. Review each header for correct values

**curl test:**
```bash
curl -I https://haide.digital 2>/dev/null | grep -i -E "content-security|x-frame|x-content|referrer|permissions|strict-transport"
```

**Report-Only mode (for testing without blocking):**
Use `Content-Security-Policy-Report-Only` header instead of `Content-Security-Policy` during testing. This logs violations without blocking resources:

```ts
{
  key: "Content-Security-Policy-Report-Only",
  value: "default-src 'self'; script-src 'self'; report-uri /api/csp-report",
}
```

---

## 5. Common Issues and Fixes

**Google Fonts blocked:**
```
Refused to load the stylesheet 'https://fonts.googleapis.com/...'
```
Fix: Ensure `style-src` includes `https://fonts.googleapis.com` and `font-src` includes `https://fonts.gstatic.com`.

**Google Analytics blocked:**
```
Refused to connect to 'https://www.google-analytics.com/...'
```
Fix: Add to `connect-src`: `https://www.google-analytics.com https://analytics.google.com`

**GTM script blocked:**
```
Refused to load the script 'https://www.googletagmanager.com/...'
```
Fix: Add to `script-src`: `https://www.googletagmanager.com`

**Three.js canvas export blocked:**
```
Refused to load the image 'blob:...'
```
Fix: Add `blob:` to `img-src`.

**Inline event handlers blocked:**
```
Refused to execute inline event handler
```
Fix: Do NOT add `'unsafe-hashes'`. Refactor inline handlers to addEventListener in JS.

**Eval blocked (GSAP/Three.js):**
```
Refused to evaluate a string as JavaScript
```
Fix: Do NOT add `'unsafe-eval'`. Check if a newer version of the library avoids eval. If unavoidable, document the exception and add `'unsafe-eval'` with a code comment explaining why.

---

## 6. Nonce-Based Script Loading (Advanced)

More secure alternative to `'unsafe-inline'`. Requires Next.js middleware.

```ts
// src/middleware.ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const nonce = Buffer.from(crypto.randomUUID()).toString("base64");
  const cspHeader = [
    "default-src 'self'",
    `script-src 'self' 'nonce-${nonce}' https://www.googletagmanager.com`,
    "style-src 'self' 'unsafe-inline' https://fonts.googleapis.com",
    "font-src 'self' https://fonts.gstatic.com",
    "img-src 'self' data: blob:",
    "connect-src 'self' https://www.google-analytics.com https://analytics.google.com",
    "frame-ancestors 'none'",
  ].join("; ");

  const requestHeaders = new Headers(request.headers);
  requestHeaders.set("x-nonce", nonce);

  const response = NextResponse.next({ request: { headers: requestHeaders } });
  response.headers.set("Content-Security-Policy", cspHeader);
  return response;
}

export const config = {
  matcher: [
    { source: "/((?!api|_next/static|_next/image|favicon.ico).*)", missing: [{ type: "header", key: "next-router-prefetch" }] },
  ],
};
```

**Note:** Nonce-based CSP is optional for Haide Digital's current scope. Implement only if removing `'unsafe-inline'` from `script-src` becomes a requirement.
