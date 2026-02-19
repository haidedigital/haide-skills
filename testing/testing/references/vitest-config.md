# Vitest Configuration

## Installation

```bash
pnpm add -D vitest @vitejs/plugin-react @testing-library/react @testing-library/user-event @testing-library/jest-dom msw jsdom @vitest/coverage-v8 @vitest/ui
```

## vitest.config.ts

```typescript
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: ['./__tests__/setup.ts'],
    include: ['__tests__/**/*.test.{ts,tsx}'],
    exclude: ['node_modules', '.next', 'e2e'],
    css: false,
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html', 'lcov'],
      include: [
        'src/lib/**',
        'src/app/api/**',
        'src/hooks/**',
        'src/components/**/!(*.stories).{ts,tsx}',
      ],
      exclude: [
        'src/components/ui/**',   // shadcn generated — not our code
        'src/components/3d/**',   // WebGL — test via E2E
        'src/components/animations/**', // GSAP wrappers — test via E2E
      ],
      thresholds: {
        'src/lib/**': {
          statements: 80,
          branches: 70,
          functions: 80,
          lines: 80,
        },
        'src/app/api/**': {
          statements: 100,
          branches: 90,
          functions: 100,
          lines: 100,
        },
      },
    },
  },
});
```

## Setup File

```typescript
// __tests__/setup.ts
import '@testing-library/jest-dom/vitest';
import { cleanup } from '@testing-library/react';
import { afterEach, beforeAll, afterAll } from 'vitest';
import { server } from './mocks/server';

// Start MSW server before all tests
beforeAll(() => server.listen({ onUnhandledRequest: 'warn' }));

// Reset handlers after each test (important for test isolation)
afterEach(() => {
  cleanup();
  server.resetHandlers();
});

// Clean up after all tests
afterAll(() => server.close());
```

## MSW Handlers

```typescript
// __tests__/mocks/handlers.ts
import { http, HttpResponse } from 'msw';

export const handlers = [
  // Contact form API
  http.post('/api/contact', async ({ request }) => {
    const body = (await request.json()) as Record<string, unknown>;

    // Honeypot check
    if (body.website) {
      return HttpResponse.json(
        { error: 'Spam detected', code: 'SPAM_DETECTED' },
        { status: 403 }
      );
    }

    // Validation
    if (!body.email || typeof body.email !== 'string') {
      return HttpResponse.json(
        { error: 'Email is required', code: 'VALIDATION_ERROR' },
        { status: 400 }
      );
    }

    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(body.email as string)) {
      return HttpResponse.json(
        { error: 'Invalid email format', code: 'VALIDATION_ERROR' },
        { status: 400 }
      );
    }

    if (!body.name || typeof body.name !== 'string') {
      return HttpResponse.json(
        { error: 'Name is required', code: 'VALIDATION_ERROR' },
        { status: 400 }
      );
    }

    if (!body.message || typeof body.message !== 'string') {
      return HttpResponse.json(
        { error: 'Message is required', code: 'VALIDATION_ERROR' },
        { status: 400 }
      );
    }

    // Success
    return HttpResponse.json({ success: true, message: 'Message sent' });
  }),
];
```

```typescript
// __tests__/mocks/server.ts
import { setupServer } from 'msw/node';
import { handlers } from './handlers';

export const server = setupServer(...handlers);
```

## Next.js Mocks

Vitest needs mocks for Next.js modules that don't exist in the test environment:

```typescript
// __tests__/mocks/next-navigation.ts
import { vi } from 'vitest';

// Mock next/navigation
vi.mock('next/navigation', () => ({
  useRouter: () => ({
    push: vi.fn(),
    replace: vi.fn(),
    back: vi.fn(),
    prefetch: vi.fn(),
  }),
  usePathname: () => '/',
  useSearchParams: () => new URLSearchParams(),
}));

// Mock next/image
vi.mock('next/image', () => ({
  default: (props: Record<string, unknown>) => {
    // eslint-disable-next-line @next/next/no-img-element
    return `<img ${Object.entries(props).map(([k, v]) => `${k}="${v}"`).join(' ')} />`;
  },
}));
```

## TypeScript Configuration

Add to `tsconfig.json` (compilerOptions):

```json
{
  "compilerOptions": {
    "types": ["vitest/globals", "@testing-library/jest-dom"]
  }
}
```

## Package.json Scripts

```json
{
  "scripts": {
    "test": "vitest run",
    "test:watch": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest run --coverage"
  }
}
```

## Troubleshooting

### "Cannot find module '@/...'" in tests
Ensure `alias` in `vitest.config.ts` matches `paths` in `tsconfig.json`.

### "ReferenceError: document is not defined"
Ensure `environment: 'jsdom'` is set. For specific files, add `// @vitest-environment jsdom` at the top.

### MSW handlers not intercepting
Check that `server.listen()` runs in `beforeAll` and `server.resetHandlers()` runs in `afterEach`. Ensure the URL in the handler matches exactly (including protocol).

### CSS/Tailwind errors in tests
`css: false` in vitest config disables CSS processing. If a component imports CSS modules directly, mock them:
```typescript
vi.mock('*.module.css', () => ({}));
```
