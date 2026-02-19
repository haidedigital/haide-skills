# Fallback Skeletons — Haide Digital

Per-section skeleton components for use as `<Suspense>` fallbacks. Each skeleton approximates the real section's layout shape using the Haide Sand shimmer pattern.

Add the shimmer utility to `globals.css` once (see SKILL.md Section 1).

---

## Hero Skeleton

```tsx
// components/skeletons/HeroSkeleton.tsx
export function HeroSkeleton() {
  return (
    <section
      className="min-h-screen bg-[#16232A] flex flex-col justify-center pt-24 px-4 md:px-8"
      aria-hidden="true"
    >
      <div className="max-w-[1560px] mx-auto w-full">
        {/* Eyebrow label */}
        <div className="skeleton h-5 w-40 rounded-full mb-6 opacity-30" />
        {/* Heading line 1 */}
        <div className="skeleton h-16 md:h-20 w-full max-w-3xl rounded-2xl mb-3 opacity-30" />
        {/* Heading line 2 */}
        <div className="skeleton h-16 md:h-20 w-3/4 max-w-2xl rounded-2xl mb-8 opacity-30" />
        {/* Subheading */}
        <div className="skeleton h-5 w-full max-w-lg rounded-lg mb-2 opacity-30" />
        <div className="skeleton h-5 w-2/3 max-w-md rounded-lg mb-10 opacity-30" />
        {/* CTA buttons */}
        <div className="flex gap-4">
          <div className="skeleton h-12 w-36 rounded-full opacity-30" />
          <div className="skeleton h-12 w-40 rounded-full opacity-30" />
        </div>
      </div>
    </section>
  );
}
```

---

## Services Section Skeleton

```tsx
// components/skeletons/ServicesSkeleton.tsx
export function ServicesSkeleton() {
  return (
    <section className="bg-[#16232A] py-20 px-4 md:px-8" aria-hidden="true">
      <div className="max-w-[1560px] mx-auto">
        {/* Section label */}
        <div className="skeleton h-4 w-28 rounded-full mb-4 opacity-30" />
        {/* Heading */}
        <div className="skeleton h-12 w-2/3 rounded-xl mb-16 opacity-30" />
        {/* Tab list + panel layout */}
        <div className="grid grid-cols-1 lg:grid-cols-[280px_1fr] gap-12">
          {/* Tab list */}
          <div className="flex flex-col gap-4">
            {Array.from({ length: 4 }).map((_, i) => (
              <div key={i} className="skeleton h-14 w-full rounded-xl opacity-30" />
            ))}
          </div>
          {/* Active panel */}
          <div className="flex flex-col gap-6">
            <div className="skeleton h-8 w-1/2 rounded-lg opacity-30" />
            <div className="skeleton h-5 w-full rounded-lg opacity-30" />
            <div className="skeleton h-5 w-5/6 rounded-lg opacity-30" />
            <div className="skeleton h-5 w-4/5 rounded-lg opacity-30" />
            <div className="grid grid-cols-2 gap-4 mt-4">
              {Array.from({ length: 4 }).map((_, i) => (
                <div key={i} className="skeleton h-20 rounded-xl opacity-30" />
              ))}
            </div>
          </div>
        </div>
      </div>
    </section>
  );
}
```

---

## Testimonials Skeleton

```tsx
// components/skeletons/TestimonialsSkeleton.tsx
export function TestimonialsSkeleton() {
  return (
    <section className="bg-[#16232A] py-20 px-4 md:px-8" aria-hidden="true">
      <div className="max-w-[1560px] mx-auto">
        {/* Heading */}
        <div className="skeleton h-10 w-64 rounded-xl mb-12 opacity-30" />
        {/* 3-card grid */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-6">
          {Array.from({ length: 3 }).map((_, i) => (
            <div key={i} className="rounded-3xl border border-white/10 p-8 flex flex-col gap-4">
              {/* Stars */}
              <div className="skeleton h-4 w-24 rounded-full opacity-30" />
              {/* Quote lines */}
              <div className="skeleton h-4 w-full rounded-lg opacity-30" />
              <div className="skeleton h-4 w-5/6 rounded-lg opacity-30" />
              <div className="skeleton h-4 w-4/5 rounded-lg opacity-30" />
              {/* Author */}
              <div className="flex items-center gap-3 mt-4">
                <div className="skeleton w-10 h-10 rounded-full opacity-30" />
                <div className="flex flex-col gap-2">
                  <div className="skeleton h-3 w-24 rounded-full opacity-30" />
                  <div className="skeleton h-3 w-32 rounded-full opacity-30" />
                </div>
              </div>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## Case Studies Skeleton

```tsx
// components/skeletons/CaseStudiesSkeleton.tsx
export function CaseStudiesSkeleton() {
  return (
    <section className="bg-[#FDFAF7] py-20 px-4 md:px-8" aria-hidden="true">
      <div className="max-w-[1560px] mx-auto">
        {/* Label */}
        <div className="skeleton h-4 w-28 rounded-full mb-4" />
        {/* Heading */}
        <div className="skeleton h-12 w-1/2 rounded-xl mb-12" />
        {/* 3-card grid */}
        <div className="grid grid-cols-1 md:grid-cols-3 gap-8">
          {Array.from({ length: 3 }).map((_, i) => (
            <div key={i} className="rounded-3xl overflow-hidden">
              {/* Image placeholder */}
              <div className="skeleton aspect-[4/3] w-full" />
              <div className="p-6 flex flex-col gap-3">
                <div className="skeleton h-5 w-3/4 rounded-lg" />
                <div className="skeleton h-4 w-full rounded-lg" />
                <div className="skeleton h-4 w-5/6 rounded-lg" />
                <div className="skeleton h-4 w-20 rounded-full mt-2" />
              </div>
            </div>
          ))}
        </div>
      </div>
    </section>
  );
}
```

---

## FAQ Section Skeleton

```tsx
// components/skeletons/FAQSkeleton.tsx
export function FAQSkeleton() {
  return (
    <section className="bg-[#FDFAF7] py-20 px-4 md:px-8" aria-hidden="true">
      <div className="max-w-[1560px] mx-auto">
        <div className="skeleton h-10 w-72 rounded-xl mb-12" />
        <div className="flex flex-col gap-0">
          {Array.from({ length: 5 }).map((_, i) => (
            <div key={i} className="border-t border-[#E4EEF0] py-7 flex items-center gap-6">
              <div className="skeleton h-6 w-8 rounded-md flex-shrink-0" />
              <div className="skeleton h-6 flex-1 rounded-lg" />
              <div className="skeleton h-6 w-6 rounded-full flex-shrink-0" />
            </div>
          ))}
          <div className="border-t border-[#E4EEF0]" />
        </div>
      </div>
    </section>
  );
}
```
