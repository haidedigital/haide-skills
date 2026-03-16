# Contact Form Security Patterns

## Table of Contents
1. Input Sanitization
2. Rate Limiting
3. Honeypot Field
4. CSRF Protection
5. Server-Side Validation
6. Origin Validation
7. Complete API Route Example

---

## 1. Input Sanitization

Strip HTML tags from all text inputs before processing. Never trust client-side sanitization alone.

```ts
// lib/sanitize.ts
function stripHtml(input: string): string {
  return input.replace(/<[^>]*>/g, "").trim();
}

function sanitizeFormData(data: Record<string, string>): Record<string, string> {
  const sanitized: Record<string, string> = {};
  for (const [key, value] of Object.entries(data)) {
    sanitized[key] = stripHtml(value);
  }
  return sanitized;
}
```

**Rules:**
- Sanitize on the server, not just the client
- Strip tags, not just escape them
- Trim whitespace from all inputs
- Enforce max length on all fields (name: 100, email: 255, message: 5000)

---

## 2. Rate Limiting

Max 3 submissions per IP per hour. Use Upstash Redis for serverless-compatible rate limiting.

```ts
// lib/rate-limit.ts
import { Ratelimit } from "@upstash/ratelimit";
import { Redis } from "@upstash/redis";

const ratelimit = new Ratelimit({
  redis: Redis.fromEnv(),
  limiter: Ratelimit.slidingWindow(3, "1 h"),
  analytics: true,
});

export async function checkRateLimit(ip: string) {
  const { success, remaining, reset } = await ratelimit.limit(ip);
  return { success, remaining, reset };
}
```

**Usage in API route:**
```ts
import { headers } from "next/headers";

const headersList = headers();
const ip = headersList.get("x-forwarded-for")?.split(",")[0] || "anonymous";
const { success } = await checkRateLimit(ip);

if (!success) {
  return NextResponse.json(
    { error: "Too many requests. Please try again later." },
    { status: 429 }
  );
}
```

**Alternative (no Redis):** Use in-memory Map with cleanup interval. Acceptable for single-instance deployments. Not suitable for multi-region Cloudflare Pages.

---

## 3. Honeypot Field

Add an invisible field that bots auto-fill. Reject submissions where it has a value.

**Frontend (hidden from users):**
```tsx
{/* Honeypot - bots will fill this, humans won't see it */}
<input
  type="text"
  name="website"
  tabIndex={-1}
  autoComplete="off"
  aria-hidden="true"
  style={{ position: "absolute", left: "-9999px", opacity: 0 }}
/>
```

**Backend (silent rejection):**
```ts
const body = await request.json();

// Honeypot check - return fake success to fool bot
if (body.website) {
  return NextResponse.json({ success: true, message: "Inquiry received." });
}
```

**Rules:**
- Never name the field "honeypot" — use realistic names like "website", "url", "fax"
- Return 200 with fake success, not 400 — bots learn from error responses
- Use CSS positioning to hide, not `display: none` (some bots skip hidden fields)
- Add `tabIndex={-1}` so keyboard users don't tab into it

---

## 4. CSRF Protection

For Next.js App Router, use origin/referer validation on API routes:

```ts
function validateOrigin(request: Request): boolean {
  const origin = request.headers.get("origin");
  const referer = request.headers.get("referer");
  const source = origin || referer;

  if (!source) return false;

  const allowedOrigins = [
    "https://haide.digital",
    "https://www.haide.digital",
  ];

  // In development, allow localhost
  if (process.env.NODE_ENV === "development") {
    allowedOrigins.push("http://localhost:3000");
  }

  return allowedOrigins.some((allowed) => source.startsWith(allowed));
}
```

**Usage:**
```ts
if (!validateOrigin(request)) {
  return NextResponse.json({ error: "Forbidden" }, { status: 403 });
}
```

**Alternative:** Next.js Server Actions include CSRF tokens automatically. Prefer Server Actions for new forms when possible.

---

## 5. Server-Side Validation

Duplicate the frontend Zod schema on the API route. Never trust client-side validation alone.

```ts
// lib/schemas/contact.ts (shared between frontend and API)
import { z } from "zod";

export const contactFormSchema = z.object({
  name: z.string().min(2).max(100).trim(),
  email: z.string().email().max(255),
  company: z.string().min(1).max(200).trim(),
  inquiryType: z.enum([
    "seo-infrastructure",
    "ai-visibility",
    "growth-audit",
    "other",
  ]),
  message: z.string().min(10).max(5000).trim(),
});

export type ContactFormValues = z.infer<typeof contactFormSchema>;
```

**API route validation:**
```ts
try {
  const body = await request.json();
  const validated = contactFormSchema.parse(body);
  // Process validated data...
} catch (error) {
  if (error instanceof z.ZodError) {
    return NextResponse.json(
      { error: "Validation failed", details: error.errors },
      { status: 400 }
    );
  }
  return NextResponse.json(
    { error: "Internal server error" },
    { status: 500 }
  );
}
```

---

## 6. Origin Validation

Validate Content-Type and HTTP method on every API route:

```ts
export async function POST(request: Request) {
  // Method is already enforced by Next.js route handler export name
  // But validate Content-Type
  const contentType = request.headers.get("content-type");
  if (!contentType?.includes("application/json")) {
    return NextResponse.json(
      { error: "Content-Type must be application/json" },
      { status: 415 }
    );
  }

  // ... rest of handler
}

// Reject other methods explicitly
export async function GET() {
  return NextResponse.json({ error: "Method not allowed" }, { status: 405 });
}
```

---

## 7. Complete API Route Example

```ts
// src/app/api/contact/route.ts
import { NextResponse } from "next/server";
import { headers } from "next/headers";
import { contactFormSchema } from "@/lib/schemas/contact";
import { checkRateLimit } from "@/lib/rate-limit";

function stripHtml(str: string): string {
  return str.replace(/<[^>]*>/g, "").trim();
}

function validateOrigin(request: Request): boolean {
  const origin = request.headers.get("origin");
  const allowed = ["https://haide.digital", "https://www.haide.digital"];
  if (process.env.NODE_ENV === "development") allowed.push("http://localhost:3000");
  return origin ? allowed.some((a) => origin.startsWith(a)) : false;
}

export async function POST(request: Request) {
  // 1. Origin check
  if (!validateOrigin(request)) {
    return NextResponse.json({ error: "Forbidden" }, { status: 403 });
  }

  // 2. Content-Type check
  if (!request.headers.get("content-type")?.includes("application/json")) {
    return NextResponse.json({ error: "Invalid content type" }, { status: 415 });
  }

  // 3. Rate limit
  const headersList = headers();
  const ip = headersList.get("x-forwarded-for")?.split(",")[0] || "anonymous";
  const { success } = await checkRateLimit(ip);
  if (!success) {
    return NextResponse.json({ error: "Too many requests" }, { status: 429 });
  }

  try {
    const body = await request.json();

    // 4. Honeypot
    if (body.website) {
      return NextResponse.json({ success: true, message: "Received." });
    }

    // 5. Validate
    const validated = contactFormSchema.parse(body);

    // 6. Sanitize
    const sanitized = {
      name: stripHtml(validated.name),
      email: validated.email,
      company: stripHtml(validated.company),
      inquiryType: validated.inquiryType,
      message: stripHtml(validated.message),
    };

    // 7. Process (send email, store in DB, etc.)
    // await sendEmail(sanitized);
    // await db.insert(contacts).values(sanitized);

    // 8. Log without PII
    console.log("Contact inquiry received", {
      inquiryType: sanitized.inquiryType,
      timestamp: new Date().toISOString(),
    });

    return NextResponse.json({ success: true, message: "Inquiry received." });
  } catch (error) {
    if (error instanceof SyntaxError) {
      return NextResponse.json({ error: "Invalid JSON" }, { status: 400 });
    }
    // Log internally, return generic to client
    console.error("Contact form error:", error);
    return NextResponse.json({ error: "Internal server error" }, { status: 500 });
  }
}

export async function GET() {
  return NextResponse.json({ error: "Method not allowed" }, { status: 405 });
}
```
