# API Error Codes — Haide Digital

Complete reference for all error codes returned by Haide Digital API routes. All API errors follow the contract:

```json
{ "error": "Human-readable description", "code": "MACHINE_READABLE_CODE" }
```

HTTP status codes are chosen semantically — never return `200` for an error.

---

## Error Code Registry

### General (all routes)

| Code | HTTP Status | Description | Client action |
|---|---|---|---|
| `INVALID_BODY` | 400 | Request body missing or not valid JSON | Re-send with valid JSON body |
| `MISSING_FIELDS` | 400 | One or more required fields absent | Show field-level validation errors |
| `INVALID_FIELD` | 400 | Field present but fails validation (format, length, etc.) | Show specific field error |
| `METHOD_NOT_ALLOWED` | 405 | HTTP method not supported by this route | Do not retry with same method |
| `RATE_LIMITED` | 429 | Too many requests from this IP/session | Show "please wait" message, retry after delay |
| `INTERNAL_ERROR` | 500 | Unhandled server exception | Show generic error message, log to Sentry |
| `SERVICE_UNAVAILABLE` | 503 | Downstream service (email, CRM) temporarily unavailable | Show "try again later" message |

---

### `/api/contact` — Contact Form Submission

| Code | HTTP Status | Description | Client-facing message |
|---|---|---|---|
| `MISSING_FIELDS` | 400 | `name`, `email`, or `message` not provided | "Please fill in all required fields." |
| `INVALID_EMAIL` | 400 | `email` fails format validation | "Please enter a valid email address." |
| `MESSAGE_TOO_SHORT` | 400 | `message` is fewer than 20 characters | "Please provide a bit more detail about your project." |
| `EMAIL_SEND_FAILED` | 502 | Email provider (Resend/Postmark) returned an error | "We couldn't send your message right now. Please try again in a moment." |
| `CRM_WRITE_FAILED` | 502 | CRM / Notion API write failed | Same as above (log separately with `SERVICE_UNAVAILABLE`) |
| `INTERNAL_ERROR` | 500 | Uncaught exception | "An unexpected error occurred. Please email us directly at hello@haide.digital" |

---

### Future routes (register when created)

| Route | Code | HTTP Status | Description |
|---|---|---|---|
| `/api/audit-request` | `INVALID_DOMAIN` | 400 | Domain URL fails URL format validation |
| `/api/audit-request` | `DUPLICATE_REQUEST` | 409 | Same email already submitted an audit request |
| `/api/newsletter` | `ALREADY_SUBSCRIBED` | 409 | Email already on list |
| `/api/newsletter` | `INVALID_EMAIL` | 400 | Email format validation failure |

---

## Error Response Examples

### 400 Bad Request

```json
HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "error": "Name and email are required",
  "code": "MISSING_FIELDS"
}
```

### 429 Rate Limited

```json
HTTP/1.1 429 Too Many Requests
Content-Type: application/json
Retry-After: 60

{
  "error": "Too many requests. Please wait before submitting again.",
  "code": "RATE_LIMITED"
}
```

### 500 Internal Error

```json
HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "error": "An unexpected error occurred. Please try again.",
  "code": "INTERNAL_ERROR"
}
```

---

## Client-Side Handling Pattern

```tsx
const handle = async (response: Response) => {
  if (response.ok) return response.json();

  const body = await response.json().catch(() => ({
    error: "An unexpected error occurred.",
    code: "INTERNAL_ERROR",
  }));

  // Map machine codes to user-friendly messages
  const messages: Record<string, string> = {
    MISSING_FIELDS: "Please fill in all required fields.",
    INVALID_EMAIL: "Please enter a valid email address.",
    MESSAGE_TOO_SHORT: "Please provide a bit more detail about your project.",
    RATE_LIMITED: "Too many requests — please wait a moment before trying again.",
    EMAIL_SEND_FAILED: "We couldn't send your message right now. Please try again shortly.",
    INTERNAL_ERROR: "An unexpected error occurred. Please email us at hello@haide.digital.",
  };

  const userMessage = messages[body.code] ?? body.error ?? messages.INTERNAL_ERROR;
  throw new Error(userMessage);
};
```
