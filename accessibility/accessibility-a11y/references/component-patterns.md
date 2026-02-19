# Accessible Component Patterns — Haide Digital

Copy-paste ready accessible implementations for every interactive component on the Haide Digital site.

---

## Navbar

```tsx
"use client";

import { useState, useEffect, useRef } from "react";
import Link from "next/link";
import Image from "next/image";

export default function Navbar() {
  const [mobileOpen, setMobileOpen] = useState(false);
  const [scrolled, setScrolled] = useState(false);
  const toggleRef = useRef<HTMLButtonElement>(null);

  useEffect(() => {
    const onScroll = () => setScrolled(window.scrollY > 60);
    onScroll();
    window.addEventListener("scroll", onScroll, { passive: true });
    return () => window.removeEventListener("scroll", onScroll);
  }, []);

  // Close on Escape, return focus to toggle button
  useEffect(() => {
    if (!mobileOpen) return;
    const onKey = (e: KeyboardEvent) => {
      if (e.key === "Escape") {
        setMobileOpen(false);
        toggleRef.current?.focus();
      }
    };
    document.addEventListener("keydown", onKey);
    return () => document.removeEventListener("keydown", onKey);
  }, [mobileOpen]);

  return (
    <nav aria-label="Main navigation" role="navigation" className="fixed z-50 ...">
      {/* ... wrapper styles ... */}

      {/* Logo */}
      <Link href="/" aria-label="Haide Digital — go to homepage">
        <Image
          src="/images/logos/haide-logo-black.png"
          alt="Haide Digital"
          width={120}
          height={34}
          className="h-7 w-auto"
          priority
        />
      </Link>

      {/* Desktop nav */}
      <div className="hidden md:flex items-center gap-8">
        <Link href="/services" className="font-poppins text-sm text-[#1A1A1A]/80 hover:text-haide-orange transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-haide-orange focus-visible:ring-offset-2 rounded">
          Services
        </Link>
        {/* ... other nav links ... */}
      </div>

      {/* Mobile toggle */}
      <button
        ref={toggleRef}
        className="md:hidden p-2 text-[#233038] focus-visible:outline-2 focus-visible:outline-haide-orange rounded"
        onClick={() => setMobileOpen(!mobileOpen)}
        aria-label={mobileOpen ? "Close navigation menu" : "Open navigation menu"}
        aria-expanded={mobileOpen}
        aria-controls="mobile-menu"
      >
        {/* SVG icon — aria-hidden since label is on the button */}
        <svg aria-hidden="true" width="24" height="24" ...>...</svg>
      </button>

      {/* Mobile menu */}
      <div
        id="mobile-menu"
        role="dialog"
        aria-modal="true"
        aria-label="Navigation menu"
        hidden={!mobileOpen}
        className="mt-2 bg-white rounded-3xl shadow-xl p-6 flex flex-col gap-4 md:hidden"
      >
        {["Services", "Industries", "Solutions", "Blog", "About"].map((item) => (
          <Link
            key={item}
            href={`/${item.toLowerCase()}`}
            className="font-poppins text-base text-haide-dark-text hover:text-haide-orange transition-colors py-1 min-h-[44px] flex items-center"
            onClick={() => setMobileOpen(false)}
          >
            {item}
          </Link>
        ))}
        <Link
          href="/contact"
          className="bg-haide-orange text-white font-poppins font-semibold text-base px-6 py-3 rounded-full text-center min-h-[44px] flex items-center justify-center"
          onClick={() => setMobileOpen(false)}
        >
          Let's Talk
        </Link>
      </div>
    </nav>
  );
}
```

---

## Skip-to-Content Link

Add as the **first child** of `<body>` in `layout.tsx`, before `<Navbar />`:

```tsx
// layout.tsx
<body>
  <a
    href="#main-content"
    className="sr-only focus:not-sr-only focus:fixed focus:top-4 focus:left-4 focus:z-[9999] focus:px-5 focus:py-3 focus:bg-haide-orange focus:text-white focus:font-poppins focus:font-semibold focus:rounded-full focus:shadow-xl"
  >
    Skip to main content
  </a>

  <Navbar />

  <main id="main-content" tabIndex={-1} className="outline-none">
    {children}
  </main>

  <Footer />
</body>
```

---

## FAQ Accordion (FAQSection)

```tsx
"use client";
import { useState } from "react";

export default function FAQSection() {
  const [openIndex, setOpenIndex] = useState<number | null>(null);

  return (
    <section className="..." aria-labelledby="faq-section-heading">
      <h2 id="faq-section-heading" className="...">
        Frequently Asked Questions
      </h2>

      <div className="..." role="list">
        {faqs.map((faq, i) => {
          const isOpen = openIndex === i;
          const triggerId = `faq-trigger-${i}`;
          const panelId = `faq-panel-${i}`;

          return (
            <div key={i} role="listitem">
              <div className="w-full h-px bg-[rgba(26,26,26,0.6)]" aria-hidden="true" />
              <button
                id={triggerId}
                className="w-full flex items-center gap-6 py-6 text-left min-h-[44px] group focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-haide-orange focus-visible:ring-offset-2 rounded"
                onClick={() => setOpenIndex(isOpen ? null : i)}
                aria-expanded={isOpen}
                aria-controls={panelId}
              >
                <span aria-hidden="true" className="font-poppins font-semibold text-haide-orange text-xl flex-shrink-0 w-8">
                  {faq.num}
                </span>
                <span className="font-poppins font-semibold text-haide-dark-text text-xl flex-1">
                  {faq.question}
                </span>
                <ChevronIcon open={isOpen} aria-hidden={true} />
              </button>

              <div
                id={panelId}
                role="region"
                aria-labelledby={triggerId}
                hidden={!isOpen}
                className="pb-6 pl-14"
              >
                <p className="font-noto text-[rgba(26,26,26,0.7)] text-base leading-relaxed">
                  {faq.answer}
                </p>
              </div>
            </div>
          );
        })}
        <div className="w-full h-px bg-[rgba(26,26,26,0.6)]" aria-hidden="true" />
      </div>
    </section>
  );
}
```

---

## Services Tabs (ServicesSection)

```tsx
"use client";
import { useState, useRef } from "react";

export default function ServicesSection() {
  const [activeTab, setActiveTab] = useState(0);
  const tabRefs = useRef<(HTMLButtonElement | null)[]>([]);

  // Arrow-key navigation between tabs
  const handleKeyDown = (e: React.KeyboardEvent, index: number) => {
    let next = index;
    if (e.key === "ArrowRight") next = (index + 1) % services.length;
    if (e.key === "ArrowLeft") next = (index - 1 + services.length) % services.length;
    if (e.key === "Home") next = 0;
    if (e.key === "End") next = services.length - 1;
    if (next !== index) {
      e.preventDefault();
      setActiveTab(next);
      tabRefs.current[next]?.focus();
    }
  };

  return (
    <section aria-labelledby="services-heading">
      <h2 id="services-heading" className="...">
        Built to compound.
      </h2>

      {/* Tab list */}
      <div role="tablist" aria-label="Haide services" className="flex flex-col gap-4">
        {services.map((s, i) => (
          <button
            key={s.name}
            ref={(el) => { tabRefs.current[i] = el; }}
            role="tab"
            id={`service-tab-${i}`}
            aria-selected={activeTab === i}
            aria-controls={`service-panel-${i}`}
            tabIndex={activeTab === i ? 0 : -1}
            onClick={() => setActiveTab(i)}
            onKeyDown={(e) => handleKeyDown(e, i)}
            className={`text-left p-4 rounded-lg transition-colors focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-haide-orange ${
              activeTab === i ? "bg-white/10 text-white" : "text-white/60 hover:text-white"
            }`}
          >
            {s.name}
          </button>
        ))}
      </div>

      {/* Tab panels */}
      {services.map((s, i) => (
        <div
          key={s.name}
          role="tabpanel"
          id={`service-panel-${i}`}
          aria-labelledby={`service-tab-${i}`}
          tabIndex={0}
          hidden={activeTab !== i}
          className="focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-haide-orange rounded-lg"
        >
          {/* panel content */}
        </div>
      ))}
    </section>
  );
}
```

---

## Testimonials Carousel

```tsx
// Carousel navigation buttons must have aria-label
<button
  onClick={scrollPrev}
  aria-label="Previous testimonial"
  className="flex items-center justify-center w-12 h-12 rounded-full bg-haide-orange/15 text-haide-orange focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-haide-orange focus-visible:ring-offset-2"
>
  <ChevronLeft aria-hidden="true" className="w-6 h-6" />
</button>

<button
  onClick={scrollNext}
  aria-label="Next testimonial"
  className="..."
>
  <ChevronRight aria-hidden="true" className="w-6 h-6" />
</button>

{/* Fix the quote render bug — use proper JSX expressions */}
<p className="font-noto text-white/60 text-base lg:text-lg leading-relaxed flex-1">
  &ldquo;{t.quote}&rdquo;
</p>
```

---

## Contact Form

```tsx
// Accessible contact form pattern
<form onSubmit={handleSubmit(onSubmit)} noValidate aria-labelledby="contact-form-heading">
  <h2 id="contact-form-heading" className="sr-only">Contact Haide Digital</h2>

  <div className="flex flex-col gap-2">
    <label htmlFor="name" className="font-poppins font-medium text-sm text-white">
      Full Name <span aria-hidden="true">*</span>
    </label>
    <input
      id="name"
      type="text"
      autoComplete="name"
      aria-required="true"
      aria-invalid={!!errors.name}
      aria-describedby={errors.name ? "name-error" : undefined}
      className="..."
      {...register("name", { required: "Name is required" })}
    />
    {errors.name && (
      <p id="name-error" role="alert" className="text-red-400 text-sm">
        {errors.name.message}
      </p>
    )}
  </div>

  {/* ... other fields ... */}

  <button
    type="submit"
    disabled={isSubmitting}
    aria-busy={isSubmitting}
    className="bg-haide-orange text-white font-poppins font-semibold px-8 py-4 rounded-full min-h-[48px] focus-visible:outline-none focus-visible:ring-2 focus-visible:ring-haide-orange focus-visible:ring-offset-2"
  >
    {isSubmitting ? "Sending..." : "Send Inquiry"}
  </button>

  {/* Success announcement */}
  {submitted && (
    <p role="status" aria-live="polite" className="text-green-400 text-base">
      Your inquiry has been sent. We'll be in touch within 24 hours.
    </p>
  )}
</form>
```

---

## Stats / Metric Numbers

Screen readers should understand the full meaning of abbreviated numerics:

```tsx
// Example from IndustriesSection or HeroSection
<div className="flex items-baseline gap-4">
  <span
    className="font-poppins font-bold text-haide-orange text-5xl leading-none"
    aria-label="142% increase in organic MRR"
  >
    +142%
  </span>
  <span aria-hidden="true" className="font-poppins font-medium text-[#1A1A1A] text-lg">
    Organic MRR
  </span>
</div>
```

---

## Route Announcer (layout.tsx)

```tsx
// components/layout/RouteAnnouncer.tsx
"use client";
import { usePathname } from "next/navigation";
import { useEffect, useRef } from "react";

export function RouteAnnouncer() {
  const pathname = usePathname();
  const ref = useRef<HTMLParagraphElement>(null);

  useEffect(() => {
    if (ref.current) {
      // Small delay lets the new page title resolve
      const timer = setTimeout(() => {
        if (ref.current) ref.current.textContent = `Navigated to ${document.title}`;
      }, 100);
      return () => clearTimeout(timer);
    }
  }, [pathname]);

  return (
    <p
      ref={ref}
      aria-live="assertive"
      aria-atomic="true"
      className="sr-only"
    />
  );
}

// In layout.tsx:
// <RouteAnnouncer />
// <Navbar />
// <main id="main-content" tabIndex={-1}>...</main>
```

---

## Global Focus Ring (globals.css)

```css
/* Add to src/app/globals.css */

/* On-brand focus ring for all interactive elements */
:focus-visible {
  outline: 2px solid #F15928;
  outline-offset: 2px;
  border-radius: 4px;
}

/* Pill-shaped elements (buttons, nav CTAs) */
button:focus-visible,
a.rounded-full:focus-visible {
  border-radius: 9999px;
}

/* Reduced motion — all components */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```
