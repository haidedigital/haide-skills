---
name: testing
description: >-
  Testing strategy and implementation for the Haide Digital website.
  Covers Vitest unit/integration tests, Playwright E2E tests, visual
  regression, and CI integration. Use when writing tests, setting up
  test infrastructure, debugging test failures, adding test coverage
  to new or existing components, or reviewing testability of code.
  Triggers on: "test", "testing", "unit test", "integration test",
  "E2E", "end-to-end", "Playwright", "Vitest", "coverage", "visual
  regression", "test CI", "test pipeline", "mock", "MSW", "test
  contact form", "test navigation".
---

# Testing — Haide Digital

## Philosophy

Test what matters for business. The Haide Digital site is a lead-generation machine — the contact form, navigation to services, and mobile experience are the critical paths. Every test must justify its existence by protecting revenue-generating functionality or preventing regressions in core user flows.

**Do not test:**
- Implementation details (internal state, private methods)
- Third-party library internals (GSAP, Three.js, Radix)
- Static markup with no logic
- CSS class names or styling (use visual regression instead)

**Always test:**
- Lead generation flow (contact form submit, validation, success/error states)
- Navigation (mobile menu open/close, scroll-to-section, route changes)
- Interactive components (accordion expand/collapse, service tabs, carousel)
- API routes (validation, error responses, rate limiting)
- Utility functions (cn(), formatters, validators)

---

## Stack

| Tool | Purpose | Config File |
|---|---|---|
| Vitest | Unit + integration tests | `vitest.config.ts` |
| @testing-library/react | Component rendering + queries | — |
| @testing-library/user-event | Realistic user interactions | — |
| MSW (Mock Service Worker) | API mocking (network-level) | `src/mocks/` |
| Playwright | E2E + visual regression | `playwright.config.ts` |
| @vitejs/plugin-react | JSX transform for Vitest | — |

### Why This Stack

- **Vitest over Jest**: Native ESM, faster HMR, compatible with Next.js 14
- **MSW over manual mocks**: Intercepts at network level, works in both tests and browser
- **Playwright over Cypress**: Cross-browser, built-in visual regression, lighter CI footprint

---

## Directory Structure

```
__tests__/
  components/
    sections/
      HeroSection.test.tsx
      ServicesSection.test.tsx
      FAQSection.test.tsx
      CTASection.test.tsx
    layout/
      Navbar.test.tsx
      Footer.test.tsx
    ui/
      Button.test.tsx
  api/
    contact.test.ts
  lib/
    utils.test.ts
  hooks/
    use-mobile.test.ts
  mocks/
    handlers.ts          # MSW request handlers
    server.ts            # MSW server setup
e2e/
  contact-flow.spec.ts   # Full lead-gen flow
  navigation.spec.ts     # Nav + scroll + routing
  service-discovery.spec.ts
  mobile.spec.ts         # Mobile-specific flows
  keyboard-nav.spec.ts   # Accessibility keyboard flows
  full-page-scroll.spec.ts
  visual/
    homepage.spec.ts     # Visual regression screenshots
```

**Naming conventions:**
- Unit/integration: `ComponentName.test.tsx` (mirrors source structure)
- E2E: `flow-name.spec.ts` (describes user journey, not component)
- Visual regression: inside `e2e/visual/` with `.spec.ts` extension

---

## Coverage Targets

| Scope | Target | Hard Minimum |
|---|---|---|
| `src/lib/` utilities | 80% | 70% |
| `src/app/api/` routes | 100% | 90% |
| `src/hooks/` | 80% | 60% |
| `src/components/` (interactive) | 60% | 40% |
| Overall | 60% | 40% |

**Do not chase coverage on:**
- Server Components with no logic (pure JSX)
- Animation wrappers (test via E2E/visual regression instead)
- Three.js/R3F components (test via E2E with WebGL fallback)

---

## Test Priority Matrix

| Priority | What | Why |
|---|---|---|
| P0 | Contact form, API route validation | Direct revenue impact |
| P1 | Navigation (mobile menu, scroll-to), FAQ accordion | Core user experience |
| P2 | Service tabs, testimonial carousel, CTA interactions | Supporting conversion |
| P3 | Static sections, footer links, decorative animations | Low regression risk |

Always write P0 and P1 tests first. P2 tests in Week 2. P3 only if time permits or a regression occurs.

---

## Vitest Setup

See `references/vitest-config.md` for the full config file and setup instructions.

**Key rules:**
- Config file: `vitest.config.ts` at project root
- Environment: `jsdom` for component tests
- Path aliases: mirror `tsconfig.json` (`@/*` maps to `src/*`)
- Setup file: `__tests__/setup.ts` (imports testing-library matchers, MSW server)
- Globals: enabled (`describe`, `it`, `expect` without imports)
- Coverage: `v8` provider, thresholds enforced

### MSW Setup

```typescript
// __tests__/mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  http.post('/api/contact', async ({ request }) => {
    const body = await request.json();
    if (!body.email) {
      return HttpResponse.json(
        { error: 'Email is required', code: 'VALIDATION_ERROR' },
        { status: 400 }
      );
    }
    return HttpResponse.json({ success: true });
  }),
];
```

```typescript
// __tests__/mocks/server.ts
import { setupServer } from 'msw/node';
import { handlers } from './handlers';
export const server = setupServer(...handlers);
```

```typescript
// __tests__/setup.ts
import '@testing-library/jest-dom/vitest';
import { server } from './mocks/server';

beforeAll(() => server.listen({ onUnhandledRequest: 'warn' }));
afterEach(() => server.resetHandlers());
afterAll(() => server.close());
```

---

## Playwright Setup

See `references/playwright-config.md` for the full config and CI setup.

**Key rules:**
- Config file: `playwright.config.ts` at project root
- Base URL: `http://localhost:3000`
- Browsers: Chromium + Mobile Safari (2 projects minimum)
- Retries: 0 locally, 2 in CI
- Screenshots: on failure (always), visual regression in `e2e/visual/`
- Web server: auto-start `pnpm dev` before tests

### Visual Regression

```typescript
// e2e/visual/homepage.spec.ts
import { test, expect } from '@playwright/test';

test('homepage matches baseline', async ({ page }) => {
  await page.goto('/');
  await page.waitForLoadState('networkidle');
  await expect(page).toHaveScreenshot('homepage-full.png', {
    fullPage: true,
    maxDiffPixelRatio: 0.001, // 0.1% threshold
  });
});
```

**Baseline management:**
- Store baselines in `e2e/visual/homepage.spec.ts-snapshots/`
- Update after intentional design changes: `pnpm test:visual:update`
- Never auto-update in CI — failures must be reviewed manually

---

## Test Patterns

See `references/test-patterns.md` for detailed examples of each pattern.

### Component Test Pattern

```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect } from 'vitest';
import { FAQSection } from '@/components/sections/FAQSection';

describe('FAQSection', () => {
  it('expands accordion item on click', async () => {
    const user = userEvent.setup();
    render(<FAQSection />);

    const trigger = screen.getByRole('button', { name: /what is organic growth/i });
    await user.click(trigger);

    expect(screen.getByRole('region')).toBeVisible();
  });
});
```

### API Route Test Pattern

```typescript
import { describe, it, expect } from 'vitest';
import { POST } from '@/app/api/contact/route';
import { NextRequest } from 'next/server';

describe('POST /api/contact', () => {
  it('rejects missing email', async () => {
    const req = new NextRequest('http://localhost/api/contact', {
      method: 'POST',
      body: JSON.stringify({ name: 'Test', message: 'Hello' }),
    });

    const res = await POST(req);
    const json = await res.json();

    expect(res.status).toBe(400);
    expect(json).toEqual({
      error: expect.any(String),
      code: 'VALIDATION_ERROR',
    });
  });
});
```

### E2E Flow Pattern

```typescript
import { test, expect } from '@playwright/test';

test('contact form submits successfully', async ({ page }) => {
  await page.goto('/contact');

  await page.fill('[name="name"]', 'Jane Smith');
  await page.fill('[name="email"]', 'jane@example.com');
  await page.fill('[name="company"]', 'Acme Corp');
  await page.fill('[name="message"]', 'Interested in growth engineering');
  await page.click('button[type="submit"]');

  await expect(page.locator('[data-testid="success-message"]')).toBeVisible();
});
```

---

## Specific Test Cases

### Contact Form (P0)

| # | Test | Type | File |
|---|---|---|---|
| 1 | Submits with all valid fields | E2E | `e2e/contact-flow.spec.ts` |
| 2 | Shows validation error for empty email | Unit | `__tests__/api/contact.test.ts` |
| 3 | Shows validation error for invalid email format | Unit | `__tests__/api/contact.test.ts` |
| 4 | Preserves input on submission error | E2E | `e2e/contact-flow.spec.ts` |
| 5 | Shows success state after submit | E2E | `e2e/contact-flow.spec.ts` |
| 6 | Handles network timeout gracefully | Unit | `__tests__/api/contact.test.ts` |
| 7 | Rejects honeypot-filled submissions | Unit | `__tests__/api/contact.test.ts` |
| 8 | Rate limits rapid submissions | Unit | `__tests__/api/contact.test.ts` |
| 9 | Returns { error, code } shape on all errors | Unit | `__tests__/api/contact.test.ts` |

### Navigation (P1)

| # | Test | Type | File |
|---|---|---|---|
| 1 | Mobile menu opens and closes | E2E | `e2e/navigation.spec.ts` |
| 2 | Menu closes on route change | E2E | `e2e/navigation.spec.ts` |
| 3 | Scroll-to-section works from nav links | E2E | `e2e/navigation.spec.ts` |
| 4 | Logo navigates to homepage | E2E | `e2e/navigation.spec.ts` |
| 5 | CTA button in nav triggers correct action | E2E | `e2e/navigation.spec.ts` |

### E2E Flows

| Flow | File | Steps |
|---|---|---|
| Contact flow | `e2e/contact-flow.spec.ts` | Land on homepage, click CTA, fill form, submit, verify success |
| Service discovery | `e2e/service-discovery.spec.ts` | Homepage, scroll to services, expand service, verify content |
| Mobile navigation | `e2e/mobile.spec.ts` | Open menu, navigate, verify scroll position, close menu |
| Keyboard navigation | `e2e/keyboard-nav.spec.ts` | Tab through nav, Enter to activate, Escape to close |
| Full page scroll | `e2e/full-page-scroll.spec.ts` | Load homepage, scroll through all sections, verify render order |

---

## Package.json Scripts

```json
{
  "test": "vitest run",
  "test:watch": "vitest",
  "test:ui": "vitest --ui",
  "test:coverage": "vitest run --coverage",
  "test:e2e": "playwright test --ignore-snapshots",
  "test:e2e:ui": "playwright test --ui",
  "test:visual": "playwright test e2e/visual/",
  "test:visual:update": "playwright test e2e/visual/ --update-snapshots",
  "test:all": "vitest run && playwright test"
}
```

---

## CI Integration

Add to the GitHub Actions pipeline (see `cicd-deployment/SKILL.md`):

```yaml
# .github/workflows/test.yml
name: Test
on: [push, pull_request]

jobs:
  unit-integration:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm
      - run: pnpm install --frozen-lockfile
      - run: pnpm test:coverage
      - uses: actions/upload-artifact@v4
        with:
          name: coverage
          path: coverage/

  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: pnpm/action-setup@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: pnpm
      - run: pnpm install --frozen-lockfile
      - run: pnpm build
      - run: npx playwright install --with-deps chromium
      - run: pnpm test:e2e
      - uses: actions/upload-artifact@v4
        if: failure()
        with:
          name: playwright-report
          path: playwright-report/
```

**CI rules:**
- Unit/integration tests run on every push and PR
- E2E tests run on every PR (not on every push to save CI minutes)
- Visual regression runs only on `main` branch pushes
- Failed E2E uploads Playwright report as artifact for debugging

---

## Implementation Order

### Week 1: Foundation

1. Install dependencies: `pnpm add -D vitest @vitejs/plugin-react @testing-library/react @testing-library/user-event @testing-library/jest-dom msw jsdom @vitest/coverage-v8`
2. Create `vitest.config.ts` (see references/vitest-config.md)
3. Create `__tests__/setup.ts` with testing-library matchers + MSW
4. Create `__tests__/mocks/handlers.ts` and `server.ts`
5. Write contact form API route tests (P0)
6. Write `src/lib/utils.test.ts`
7. Add `test`, `test:watch`, `test:coverage` scripts to package.json

### Week 2: E2E

1. Install Playwright: `pnpm add -D @playwright/test && npx playwright install`
2. Create `playwright.config.ts` (see references/playwright-config.md)
3. Write `e2e/contact-flow.spec.ts`
4. Write `e2e/navigation.spec.ts`
5. Write `e2e/mobile.spec.ts`
6. Add `test:e2e`, `test:e2e:ui` scripts to package.json
7. Add CI workflow

### Post-Launch

1. Set up visual regression baselines
2. Write remaining P2/P3 tests
3. Add `test:visual`, `test:visual:update` scripts
4. Enable visual regression in CI on `main` branch

---

## Verification Checklist

Before marking testing implementation complete:

- [ ] `pnpm test` runs and passes
- [ ] `pnpm test:coverage` meets threshold targets
- [ ] `pnpm test:e2e` passes on Chromium + Mobile Safari
- [ ] Contact form test covers submit, validation, error, and success
- [ ] Navigation test covers mobile menu and scroll-to-section
- [ ] CI pipeline runs tests on PR
- [ ] No `any` types in test files
- [ ] MSW handlers match actual API route contracts
- [ ] All test files follow naming conventions

---

## References

- `references/vitest-config.md` — Full Vitest configuration with path aliases, coverage thresholds, and setup
- `references/playwright-config.md` — Full Playwright configuration with browser projects, visual regression settings, and CI optimizations
- `references/test-patterns.md` — Detailed test examples for every component type: sections, forms, API routes, hooks, utilities, E2E flows
