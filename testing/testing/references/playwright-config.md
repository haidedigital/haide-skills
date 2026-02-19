# Playwright Configuration

## Installation

```bash
pnpm add -D @playwright/test
npx playwright install chromium webkit
```

Only install Chromium + WebKit (Safari). Firefox is optional and not required for the Haide site's target audience.

## playwright.config.ts

```typescript
import { defineConfig, devices } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 1 : undefined,
  reporter: process.env.CI
    ? [['html', { open: 'never' }], ['github']]
    : [['html', { open: 'on-failure' }]],

  use: {
    baseURL: 'http://localhost:3000',
    trace: 'on-first-retry',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
  },

  projects: [
    {
      name: 'Desktop Chrome',
      use: { ...devices['Desktop Chrome'] },
    },
    {
      name: 'Mobile Safari',
      use: { ...devices['iPhone 13'] },
    },
  ],

  webServer: {
    command: 'pnpm dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
    timeout: 120_000,
  },
});
```

## Visual Regression Configuration

Visual regression tests live in `e2e/visual/` and use Playwright's built-in screenshot comparison.

### Key Settings

```typescript
// In playwright.config.ts, add a visual regression project
{
  name: 'Visual Regression',
  use: {
    ...devices['Desktop Chrome'],
    // Consistent viewport for reproducible screenshots
    viewport: { width: 1440, height: 900 },
  },
  testDir: './e2e/visual',
  // Slower timeout for full-page screenshots
  timeout: 60_000,
}
```

### Screenshot Options

```typescript
await expect(page).toHaveScreenshot('section-name.png', {
  fullPage: true,                // Capture entire scrollable page
  maxDiffPixelRatio: 0.001,      // 0.1% pixel difference allowed
  threshold: 0.2,                // Per-pixel color threshold (0-1)
  animations: 'disabled',        // Disable CSS/GSAP animations for consistency
});
```

### Section-Level Screenshots

Instead of one massive full-page screenshot, capture individual sections for more targeted regression detection:

```typescript
test('hero section matches baseline', async ({ page }) => {
  await page.goto('/');
  await page.waitForLoadState('networkidle');

  const hero = page.locator('section').first();
  await expect(hero).toHaveScreenshot('hero-section.png', {
    maxDiffPixelRatio: 0.001,
    animations: 'disabled',
  });
});

test('services section matches baseline', async ({ page }) => {
  await page.goto('/#how-it-works');
  await page.waitForLoadState('networkidle');

  const services = page.locator('#how-it-works');
  await expect(services).toHaveScreenshot('services-section.png', {
    maxDiffPixelRatio: 0.001,
    animations: 'disabled',
  });
});
```

### Baseline Management

- Baselines stored in `e2e/visual/*.spec.ts-snapshots/`
- Each platform/browser gets its own baseline directory
- **Update command**: `pnpm test:visual:update`
- **Never auto-update in CI** — review diffs manually
- Commit baselines to git (they are the source of truth)

### Dealing with Dynamic Content

For sections with dynamic/random content (testimonial carousel, time-based greetings):

```typescript
// Mock the clock for time-dependent content
await page.clock.setFixedTime(new Date('2025-06-15T10:00:00'));

// Wait for animations to complete before screenshot
await page.waitForTimeout(2000);

// Or wait for a specific element that signals "ready"
await page.waitForSelector('[data-animation-complete="true"]');
```

## CI Configuration

### GitHub Actions

```yaml
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
        retention-days: 7

visual-regression:
  runs-on: ubuntu-latest
  if: github.ref == 'refs/heads/main'
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
    - run: pnpm test:visual
    - uses: actions/upload-artifact@v4
      if: failure()
      with:
        name: visual-diff-report
        path: playwright-report/
        retention-days: 14
```

### CI Optimization Tips

1. **Cache Playwright browsers**: Use `actions/cache` with `~/.cache/ms-playwright` as key
2. **Only install needed browsers**: `npx playwright install --with-deps chromium` (skip webkit in CI)
3. **Use `pnpm build`** instead of dev server for faster, more stable E2E
4. **Set `workers: 1` in CI** to avoid flakiness from parallel tests on shared resources

## Package.json Scripts

```json
{
  "scripts": {
    "test:e2e": "playwright test --ignore-snapshots",
    "test:e2e:ui": "playwright test --ui",
    "test:visual": "playwright test e2e/visual/",
    "test:visual:update": "playwright test e2e/visual/ --update-snapshots"
  }
}
```

## Troubleshooting

### Tests fail with "page.goto: net::ERR_CONNECTION_REFUSED"
The dev server didn't start. Check that `pnpm dev` works and port 3000 is free. Increase `webServer.timeout` if the build is slow.

### Visual regression fails in CI but passes locally
Different OS renders fonts differently. Either:
- Use Docker with a consistent base image
- Increase `maxDiffPixelRatio` to `0.005` (0.5%)
- Generate separate baselines per OS

### Flaky mobile tests
Mobile Safari emulation may behave differently from real devices. For critical mobile flows, test on actual devices using Playwright's `connect` API or a device farm.

### GSAP animations causing inconsistent screenshots
Add `animations: 'disabled'` to screenshot options. This pauses CSS animations. For GSAP, inject a script to kill all timelines:
```typescript
await page.evaluate(() => {
  if (window.gsap) {
    window.gsap.globalTimeline.pause();
  }
});
```
