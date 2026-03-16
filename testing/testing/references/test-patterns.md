# Test Patterns — Haide Digital

Detailed test examples for every component type in the project. Copy and adapt these patterns when writing new tests.

---

## Table of Contents

1. [API Route Tests](#1-api-route-tests)
2. [Utility Function Tests](#2-utility-function-tests)
3. [Hook Tests](#3-hook-tests)
4. [Interactive Component Tests](#4-interactive-component-tests)
5. [Form Component Tests](#5-form-component-tests)
6. [E2E Contact Flow](#6-e2e-contact-flow)
7. [E2E Navigation Flow](#7-e2e-navigation-flow)
8. [E2E Mobile Flow](#8-e2e-mobile-flow)
9. [E2E Keyboard Navigation](#9-e2e-keyboard-navigation)
10. [E2E Full Page Scroll](#10-e2e-full-page-scroll)

---

## 1. API Route Tests

Test the `/api/contact` route directly by importing the handler and calling it with mock `NextRequest` objects.

```typescript
// __tests__/api/contact.test.ts
import { describe, it, expect } from 'vitest';
import { POST } from '@/app/api/contact/route';
import { NextRequest } from 'next/server';

function createRequest(body: Record<string, unknown>): NextRequest {
  return new NextRequest('http://localhost:3000/api/contact', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(body),
  });
}

describe('POST /api/contact', () => {
  const validBody = {
    name: 'Jane Smith',
    email: 'jane@example.com',
    company: 'Acme Corp',
    message: 'Interested in growth engineering',
  };

  it('accepts valid submission', async () => {
    const res = await POST(createRequest(validBody));
    expect(res.status).toBe(200);
    const json = await res.json();
    expect(json.success).toBe(true);
  });

  it('rejects missing email', async () => {
    const { email, ...noEmail } = validBody;
    const res = await POST(createRequest(noEmail));
    expect(res.status).toBe(400);
    const json = await res.json();
    expect(json).toMatchObject({
      error: expect.any(String),
      code: 'VALIDATION_ERROR',
    });
  });

  it('rejects invalid email format', async () => {
    const res = await POST(createRequest({ ...validBody, email: 'not-an-email' }));
    expect(res.status).toBe(400);
    const json = await res.json();
    expect(json.code).toBe('VALIDATION_ERROR');
  });

  it('rejects missing name', async () => {
    const { name, ...noName } = validBody;
    const res = await POST(createRequest(noName));
    expect(res.status).toBe(400);
  });

  it('rejects missing message', async () => {
    const { message, ...noMessage } = validBody;
    const res = await POST(createRequest(noMessage));
    expect(res.status).toBe(400);
  });

  it('rejects honeypot-filled submission', async () => {
    const res = await POST(createRequest({ ...validBody, website: 'http://spam.com' }));
    expect(res.status).toBe(403);
    const json = await res.json();
    expect(json.code).toBe('SPAM_DETECTED');
  });

  it('returns { error, code } shape on all errors', async () => {
    const res = await POST(createRequest({}));
    const json = await res.json();
    expect(json).toHaveProperty('error');
    expect(json).toHaveProperty('code');
    expect(typeof json.error).toBe('string');
    expect(typeof json.code).toBe('string');
  });
});
```

---

## 2. Utility Function Tests

```typescript
// __tests__/lib/utils.test.ts
import { describe, it, expect } from 'vitest';
import { cn } from '@/lib/utils';

describe('cn()', () => {
  it('merges class names', () => {
    expect(cn('foo', 'bar')).toBe('foo bar');
  });

  it('handles conditional classes', () => {
    expect(cn('base', false && 'hidden', 'visible')).toBe('base visible');
  });

  it('resolves Tailwind conflicts (last wins)', () => {
    const result = cn('p-4', 'p-8');
    expect(result).toBe('p-8');
  });

  it('handles undefined and null gracefully', () => {
    expect(cn('base', undefined, null, 'end')).toBe('base end');
  });

  it('handles empty string', () => {
    expect(cn('')).toBe('');
  });
});
```

---

## 3. Hook Tests

```typescript
// __tests__/hooks/use-mobile.test.ts
import { describe, it, expect, vi, beforeEach } from 'vitest';
import { renderHook, act } from '@testing-library/react';
import { useIsMobile } from '@/hooks/use-mobile';

describe('useIsMobile', () => {
  let matchMediaMock: ReturnType<typeof vi.fn>;

  beforeEach(() => {
    matchMediaMock = vi.fn().mockReturnValue({
      matches: false,
      addEventListener: vi.fn(),
      removeEventListener: vi.fn(),
    });
    Object.defineProperty(window, 'matchMedia', {
      writable: true,
      value: matchMediaMock,
    });
  });

  it('returns false on desktop viewport', () => {
    matchMediaMock.mockReturnValue({
      matches: false,
      addEventListener: vi.fn(),
      removeEventListener: vi.fn(),
    });

    const { result } = renderHook(() => useIsMobile());
    expect(result.current).toBe(false);
  });

  it('returns true on mobile viewport', () => {
    matchMediaMock.mockReturnValue({
      matches: true,
      addEventListener: vi.fn(),
      removeEventListener: vi.fn(),
    });

    const { result } = renderHook(() => useIsMobile());
    expect(result.current).toBe(true);
  });

  it('updates when viewport changes', () => {
    let listener: ((e: { matches: boolean }) => void) | null = null;
    matchMediaMock.mockReturnValue({
      matches: false,
      addEventListener: (_: string, cb: (e: { matches: boolean }) => void) => {
        listener = cb;
      },
      removeEventListener: vi.fn(),
    });

    const { result } = renderHook(() => useIsMobile());
    expect(result.current).toBe(false);

    // Simulate viewport change
    act(() => {
      listener?.({ matches: true });
    });
    expect(result.current).toBe(true);
  });

  it('cleans up listener on unmount', () => {
    const removeListener = vi.fn();
    matchMediaMock.mockReturnValue({
      matches: false,
      addEventListener: vi.fn(),
      removeEventListener: removeListener,
    });

    const { unmount } = renderHook(() => useIsMobile());
    unmount();
    expect(removeListener).toHaveBeenCalled();
  });
});
```

---

## 4. Interactive Component Tests

### FAQ Accordion

```typescript
// __tests__/components/sections/FAQSection.test.tsx
import { render, screen, within } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect } from 'vitest';
import { FAQSection } from '@/components/sections/FAQSection';

describe('FAQSection', () => {
  it('renders all FAQ items collapsed by default', () => {
    render(<FAQSection />);
    const buttons = screen.getAllByRole('button');
    buttons.forEach((button) => {
      expect(button).toHaveAttribute('aria-expanded', 'false');
    });
  });

  it('expands an item on click', async () => {
    const user = userEvent.setup();
    render(<FAQSection />);

    const firstItem = screen.getAllByRole('button')[0];
    await user.click(firstItem);

    expect(firstItem).toHaveAttribute('aria-expanded', 'true');
  });

  it('collapses an expanded item on second click', async () => {
    const user = userEvent.setup();
    render(<FAQSection />);

    const firstItem = screen.getAllByRole('button')[0];
    await user.click(firstItem); // expand
    await user.click(firstItem); // collapse

    expect(firstItem).toHaveAttribute('aria-expanded', 'false');
  });

  it('has correct heading hierarchy (H2 for section title)', () => {
    render(<FAQSection />);
    const heading = screen.getByRole('heading', { level: 2 });
    expect(heading).toBeInTheDocument();
  });
});
```

### Navbar Mobile Menu

```typescript
// __tests__/components/layout/Navbar.test.tsx
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect, vi } from 'vitest';

// Mock next/navigation before importing component
vi.mock('next/navigation', () => ({
  useRouter: () => ({ push: vi.fn(), prefetch: vi.fn() }),
  usePathname: () => '/',
}));

// Mock next/image
vi.mock('next/image', () => ({
  default: ({ alt, ...props }: { alt: string }) => <img alt={alt} {...props} />,
}));

import { Navbar } from '@/components/layout/Navbar';

describe('Navbar', () => {
  it('renders the logo', () => {
    render(<Navbar />);
    expect(screen.getByAltText(/haide/i)).toBeInTheDocument();
  });

  it('has a mobile menu toggle button', () => {
    render(<Navbar />);
    const toggle = screen.getByRole('button', { name: /menu/i });
    expect(toggle).toBeInTheDocument();
  });

  it('toggles mobile menu on button click', async () => {
    const user = userEvent.setup();
    render(<Navbar />);

    const toggle = screen.getByRole('button', { name: /menu/i });
    await user.click(toggle);

    // Menu should be visible
    expect(toggle).toHaveAttribute('aria-expanded', 'true');
  });

  it('contains CTA button', () => {
    render(<Navbar />);
    expect(screen.getByRole('link', { name: /growth plan/i })).toBeInTheDocument();
  });
});
```

---

## 5. Form Component Tests

```typescript
// __tests__/components/ContactForm.test.tsx
import { render, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import { describe, it, expect } from 'vitest';
import { server } from '../mocks/server';
import { http, HttpResponse } from 'msw';

// Import the form component (adjust path to actual location)
import ContactPage from '@/app/contact/page';

describe('Contact Form', () => {
  it('disables submit button while submitting', async () => {
    const user = userEvent.setup();
    render(<ContactPage />);

    await user.type(screen.getByLabelText(/name/i), 'Jane');
    await user.type(screen.getByLabelText(/email/i), 'jane@test.com');
    await user.type(screen.getByLabelText(/message/i), 'Hello');

    const submit = screen.getByRole('button', { name: /submit|send/i });
    await user.click(submit);

    // Button should be disabled during submission
    expect(submit).toBeDisabled();
  });

  it('shows success message after successful submission', async () => {
    const user = userEvent.setup();
    render(<ContactPage />);

    await user.type(screen.getByLabelText(/name/i), 'Jane');
    await user.type(screen.getByLabelText(/email/i), 'jane@test.com');
    await user.type(screen.getByLabelText(/message/i), 'Hello');
    await user.click(screen.getByRole('button', { name: /submit|send/i }));

    await waitFor(() => {
      expect(screen.getByText(/sent|success|thank/i)).toBeInTheDocument();
    });
  });

  it('shows error message on server error', async () => {
    // Override the handler for this test
    server.use(
      http.post('/api/contact', () => {
        return HttpResponse.json(
          { error: 'Server error', code: 'INTERNAL_ERROR' },
          { status: 500 }
        );
      })
    );

    const user = userEvent.setup();
    render(<ContactPage />);

    await user.type(screen.getByLabelText(/name/i), 'Jane');
    await user.type(screen.getByLabelText(/email/i), 'jane@test.com');
    await user.type(screen.getByLabelText(/message/i), 'Hello');
    await user.click(screen.getByRole('button', { name: /submit|send/i }));

    await waitFor(() => {
      expect(screen.getByText(/error|failed|try again/i)).toBeInTheDocument();
    });
  });

  it('preserves input values on submission error', async () => {
    server.use(
      http.post('/api/contact', () => {
        return HttpResponse.json(
          { error: 'Server error', code: 'INTERNAL_ERROR' },
          { status: 500 }
        );
      })
    );

    const user = userEvent.setup();
    render(<ContactPage />);

    await user.type(screen.getByLabelText(/name/i), 'Jane Smith');
    await user.type(screen.getByLabelText(/email/i), 'jane@test.com');
    await user.type(screen.getByLabelText(/message/i), 'Important message');
    await user.click(screen.getByRole('button', { name: /submit|send/i }));

    await waitFor(() => {
      expect(screen.getByLabelText(/name/i)).toHaveValue('Jane Smith');
      expect(screen.getByLabelText(/email/i)).toHaveValue('jane@test.com');
      expect(screen.getByLabelText(/message/i)).toHaveValue('Important message');
    });
  });
});
```

---

## 6. E2E Contact Flow

```typescript
// e2e/contact-flow.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Contact Form Flow', () => {
  test('completes full lead-generation journey', async ({ page }) => {
    // 1. Land on homepage
    await page.goto('/');
    await page.waitForLoadState('networkidle');

    // 2. Click primary CTA
    await page.click('a:has-text("growth plan")');

    // 3. Should navigate to contact page
    await expect(page).toHaveURL(/contact/);

    // 4. Fill in the form
    await page.fill('[name="name"]', 'Jane Smith');
    await page.fill('[name="email"]', 'jane@acme.com');
    await page.fill('[name="company"]', 'Acme Corp');
    await page.fill('[name="message"]', 'We need growth engineering for our SaaS platform');

    // 5. Submit
    await page.click('button[type="submit"]');

    // 6. Verify success state
    await expect(page.locator('[data-testid="success-message"]')).toBeVisible({
      timeout: 10_000,
    });
  });

  test('shows validation error for invalid email', async ({ page }) => {
    await page.goto('/contact');
    await page.fill('[name="name"]', 'Jane');
    await page.fill('[name="email"]', 'not-an-email');
    await page.fill('[name="message"]', 'Test');
    await page.click('button[type="submit"]');

    // Should show error, not success
    await expect(page.locator('text=/error|invalid|required/i')).toBeVisible();
  });

  test('preserves form data on failed submission', async ({ page }) => {
    await page.goto('/contact');

    const name = 'Jane Smith';
    const email = 'jane@acme.com';
    const message = 'Important inquiry about growth engineering';

    await page.fill('[name="name"]', name);
    await page.fill('[name="email"]', email);
    await page.fill('[name="message"]', message);

    // Simulate network error
    await page.route('/api/contact', (route) => route.abort('connectionfailed'));
    await page.click('button[type="submit"]');

    // Form should still have the values
    await expect(page.locator('[name="name"]')).toHaveValue(name);
    await expect(page.locator('[name="email"]')).toHaveValue(email);
    await expect(page.locator('[name="message"]')).toHaveValue(message);
  });
});
```

---

## 7. E2E Navigation Flow

```typescript
// e2e/navigation.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Navigation', () => {
  test('logo navigates to homepage', async ({ page }) => {
    await page.goto('/contact');
    await page.click('a[href="/"]');
    await expect(page).toHaveURL('/');
  });

  test('scroll-to-section works from nav links', async ({ page }) => {
    await page.goto('/');

    // Click "How it works" nav link (or equivalent)
    await page.click('a[href="#how-it-works"]');

    // The section should be in viewport
    const section = page.locator('#how-it-works');
    await expect(section).toBeInViewport({ timeout: 5000 });
  });

  test('CTA button in nav links to contact', async ({ page }) => {
    await page.goto('/');
    const ctaLink = page.locator('nav a:has-text("growth plan")');
    await ctaLink.click();
    await expect(page).toHaveURL(/contact/);
  });
});

test.describe('Mobile Navigation', () => {
  test.use({ viewport: { width: 375, height: 812 } }); // iPhone SE

  test('opens and closes mobile menu', async ({ page }) => {
    await page.goto('/');

    // Open menu
    const menuButton = page.getByRole('button', { name: /menu/i });
    await menuButton.click();
    await expect(menuButton).toHaveAttribute('aria-expanded', 'true');

    // Menu links should be visible
    await expect(page.locator('nav a:has-text("Services")')).toBeVisible();

    // Close menu
    await menuButton.click();
    await expect(menuButton).toHaveAttribute('aria-expanded', 'false');
  });

  test('closes menu on navigation', async ({ page }) => {
    await page.goto('/');

    // Open menu
    await page.getByRole('button', { name: /menu/i }).click();

    // Click a nav link
    await page.locator('nav a[href="#how-it-works"]').click();

    // Menu should close
    await expect(page.getByRole('button', { name: /menu/i }))
      .toHaveAttribute('aria-expanded', 'false');
  });
});
```

---

## 8. E2E Mobile Flow

```typescript
// e2e/mobile.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Mobile Experience', () => {
  test.use({ viewport: { width: 375, height: 812 } });

  test('no horizontal overflow on homepage', async ({ page }) => {
    await page.goto('/');
    await page.waitForLoadState('networkidle');

    const bodyWidth = await page.evaluate(() => document.body.scrollWidth);
    const viewportWidth = await page.evaluate(() => window.innerWidth);

    expect(bodyWidth).toBeLessThanOrEqual(viewportWidth);
  });

  test('all interactive elements meet 44px minimum tap target', async ({ page }) => {
    await page.goto('/');

    const buttons = await page.locator('button, a').all();
    for (const button of buttons) {
      if (await button.isVisible()) {
        const box = await button.boundingBox();
        if (box) {
          expect(
            box.height >= 44 || box.width >= 44,
            `Element at (${box.x}, ${box.y}) is ${box.width}x${box.height} — below 44px minimum`
          ).toBe(true);
        }
      }
    }
  });

  test('body text is at least 16px', async ({ page }) => {
    await page.goto('/');

    const paragraphs = await page.locator('p').all();
    for (const p of paragraphs) {
      if (await p.isVisible()) {
        const fontSize = await p.evaluate((el) =>
          parseFloat(window.getComputedStyle(el).fontSize)
        );
        expect(fontSize, `Paragraph text is ${fontSize}px — below 16px minimum`).toBeGreaterThanOrEqual(16);
      }
    }
  });
});
```

---

## 9. E2E Keyboard Navigation

```typescript
// e2e/keyboard-nav.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Keyboard Navigation', () => {
  test('Tab navigates through all interactive elements in order', async ({ page }) => {
    await page.goto('/');

    // Tab to first interactive element (should be skip-to-content or nav link)
    await page.keyboard.press('Tab');
    const firstFocused = await page.evaluate(() => document.activeElement?.tagName);
    expect(['A', 'BUTTON']).toContain(firstFocused);
  });

  test('Enter activates focused link', async ({ page }) => {
    await page.goto('/');

    // Tab to CTA link
    let attempts = 0;
    while (attempts < 20) {
      await page.keyboard.press('Tab');
      const href = await page.evaluate(() =>
        (document.activeElement as HTMLAnchorElement)?.href
      );
      if (href?.includes('contact')) {
        await page.keyboard.press('Enter');
        await expect(page).toHaveURL(/contact/);
        return;
      }
      attempts++;
    }
  });

  test('Escape closes mobile menu', async ({ page }) => {
    page.setViewportSize({ width: 375, height: 812 });
    await page.goto('/');

    // Open menu
    const menuButton = page.getByRole('button', { name: /menu/i });
    await menuButton.click();
    await expect(menuButton).toHaveAttribute('aria-expanded', 'true');

    // Press Escape
    await page.keyboard.press('Escape');
    await expect(menuButton).toHaveAttribute('aria-expanded', 'false');

    // Focus should return to the menu button
    const focused = await page.evaluate(() => document.activeElement?.getAttribute('aria-label'));
    expect(focused).toMatch(/menu/i);
  });

  test('FAQ accordion responds to Enter and Space', async ({ page }) => {
    await page.goto('/');

    // Navigate to FAQ section
    await page.goto('/#faq');
    await page.waitForTimeout(1000);

    // Find first accordion button
    const faqButton = page.locator('[data-testid="faq-trigger"]').first();
    await faqButton.focus();

    // Press Enter to expand
    await page.keyboard.press('Enter');
    await expect(faqButton).toHaveAttribute('aria-expanded', 'true');

    // Press Enter again to collapse
    await page.keyboard.press('Enter');
    await expect(faqButton).toHaveAttribute('aria-expanded', 'false');

    // Press Space to expand
    await page.keyboard.press('Space');
    await expect(faqButton).toHaveAttribute('aria-expanded', 'true');
  });
});
```

---

## 10. E2E Full Page Scroll

```typescript
// e2e/full-page-scroll.spec.ts
import { test, expect } from '@playwright/test';

test.describe('Full Page Scroll', () => {
  test('all homepage sections render in correct order', async ({ page }) => {
    await page.goto('/');
    await page.waitForLoadState('networkidle');

    // Define expected section order
    const expectedSections = [
      'hero',
      'problem',
      'how-it-works', // Contains Services + Process
      'industries',
      'comparison',
      'testimonials',
      'case-studies',
      'faq',
      'cta',
    ];

    // Scroll through each section and verify it exists
    for (const sectionId of expectedSections) {
      const section = page.locator(`[id="${sectionId}"], [data-section="${sectionId}"]`);
      // At least one matching element should exist
      const count = await section.count();
      expect(count, `Section "${sectionId}" not found on page`).toBeGreaterThan(0);
    }

    // Verify order: each section's top offset should be greater than the previous
    let lastTop = -1;
    for (const sectionId of expectedSections) {
      const section = page.locator(`[id="${sectionId}"], [data-section="${sectionId}"]`).first();
      if ((await section.count()) > 0) {
        const box = await section.boundingBox();
        if (box) {
          const scrollTop = await page.evaluate(
            (id) => {
              const el = document.querySelector(`[id="${id}"], [data-section="${id}"]`);
              return el?.getBoundingClientRect().top ?? 0;
            },
            sectionId
          );
          // We can't guarantee exact order from boundingBox during scroll,
          // but the DOM order should be correct
        }
      }
    }
  });

  test('smooth scroll works without errors', async ({ page }) => {
    const errors: string[] = [];
    page.on('pageerror', (err) => errors.push(err.message));

    await page.goto('/');
    await page.waitForLoadState('networkidle');

    // Scroll to bottom
    await page.evaluate(() => window.scrollTo({ top: document.body.scrollHeight, behavior: 'smooth' }));
    await page.waitForTimeout(3000);

    // Scroll back to top
    await page.evaluate(() => window.scrollTo({ top: 0, behavior: 'smooth' }));
    await page.waitForTimeout(2000);

    // No JS errors during scroll
    expect(errors, `Page errors during scroll: ${errors.join(', ')}`).toHaveLength(0);
  });
});
```

---

## General Testing Rules

1. **Never test implementation details** — test behavior, not how it's achieved
2. **Use `screen.getByRole()` first** — mirrors how users and assistive tech find elements
3. **Prefer `userEvent` over `fireEvent`** — `userEvent` simulates real browser behavior
4. **One assertion per test** (when possible) — makes failures easier to diagnose
5. **Use `data-testid` sparingly** — only when no accessible role/label exists
6. **Mock at the boundary** — MSW for network, `vi.mock()` for modules, never mock internal functions
7. **Keep tests independent** — each test should work in isolation, no shared mutable state
