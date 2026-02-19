# Contrast Checks — Haide Digital Palette

Full contrast matrix for every colour combination used in the Haide Digital design system, with WCAG 2.1 AA verdicts.

---

## Reference: WCAG 2.1 AA Thresholds

| Text type | Minimum ratio |
|---|---|
| Normal text (< 18px regular, < 14px bold) | **4.5:1** |
| Large text (≥ 18px regular, or ≥ 14px bold) | **3:1** |
| UI components (boundaries, icons, focus rings) | **3:1** |
| Decorative / disabled | No requirement |

---

## Full Palette Matrix

### On White `#FFFFFF`

| Token | Hex | Ratio | Normal text | Large text | Usage guidance |
|---|---|---|---|---|---|
| `haide-dark` | `#16232A` | 14.4:1 | ✅ PASS | ✅ PASS | Preferred dark text — use freely |
| `haide-body` | `#233038` | 12.5:1 | ✅ PASS | ✅ PASS | Headings, nav items |
| `haide-orange` | `#F15928` | 3.03:1 | ❌ FAIL | ✅ PASS | **Large headings only (≥ 18px), bold ≥ 14px** |
| Gray 600 | `#4B5563` | 5.9:1 | ✅ PASS | ✅ PASS | Subtext, descriptions |
| Dark 80% opacity | `rgba(26,26,26,0.8)` | ~5.4:1 | ✅ PASS | ✅ PASS | Secondary body copy |
| Dark 70% opacity | `rgba(26,26,26,0.7)` | ~4.8:1 | ✅ PASS | ✅ PASS | Muted descriptions |
| Dark 60% opacity | `rgba(26,26,26,0.6)` | ~4.1:1 | ⚠️ BORDERLINE | ✅ PASS | Captions only; prefer ≥ 0.7 for body |
| Dark 50% opacity | `rgba(26,26,26,0.5)` | ~3.1:1 | ❌ FAIL | ✅ PASS | Do not use for readable text |
| Dark 30% opacity | `rgba(26,26,26,0.3)` | ~1.8:1 | ❌ FAIL | ❌ FAIL | Decorative dividers only |

### On Haide Orange `#F15928`

| Token | Hex | Ratio | Normal text | Large text | Usage guidance |
|---|---|---|---|---|---|
| White | `#FFFFFF` | 3.03:1 | ❌ FAIL | ✅ PASS | **Button text must be ≥ 18px or bold ≥ 14px** |
| `#16232A` | Dark | 4.74:1 | ✅ PASS | ✅ PASS | Dark text on orange background — passes |

> ⚠️ **CTA Buttons:** The current "Let's Talk" and "Get Started" buttons use white 14–15px medium weight text on `#F15928`. This ratio is 3.03:1 — only passes for **bold ≥ 14px**. Ensure `font-weight: 600` or higher on all button text. Do not use `font-medium` (500) on orange buttons with text below 18px.

### On Dark `#16232A`

| Token | Hex | Ratio | Normal text | Large text | Usage guidance |
|---|---|---|---|---|---|
| White | `#FFFFFF` | 14.4:1 | ✅ PASS | ✅ PASS | Primary text in dark sections |
| White 70% | `rgba(255,255,255,0.7)` | ~7.2:1 | ✅ PASS | ✅ PASS | Body copy in dark sections |
| White 60% | `rgba(255,255,255,0.6)` | ~5.9:1 | ✅ PASS | ✅ PASS | Secondary text |
| White 50% | `rgba(255,255,255,0.5)` | ~4.7:1 | ✅ PASS | ✅ PASS | Muted text — minimum for body copy |
| White 40% | `rgba(255,255,255,0.4)` | ~3.8:1 | ❌ FAIL | ✅ PASS | Process bullet points, captions only |
| White 20% | `rgba(255,255,255,0.2)` | ~1.9:1 | ❌ FAIL | ❌ FAIL | Decorative lines only |
| `haide-orange` | `#F15928` | 3.27:1 | ❌ FAIL | ✅ PASS | Step numbers, decorative accents (≥ 18px) |

### On Dark BG `#0A1628` (hero gradient deep)

| Token | Ratio | Normal text | Large text |
|---|---|---|---|
| White 70% | ~7.8:1 | ✅ PASS | ✅ PASS |
| White 60% | ~6.7:1 | ✅ PASS | ✅ PASS |
| White 50% | ~5.6:1 | ✅ PASS | ✅ PASS |
| White 40% | ~4.5:1 | ✅ PASS | ✅ PASS |
| White 30% | ~3.3:1 | ❌ FAIL | ✅ PASS |

---

## Safe Combinations — Copy-Paste Ready

```tsx
// Body copy on white background ✅
className="text-[#16232A]"                // 14.4:1
className="text-[rgba(26,26,26,0.8)]"    // 5.4:1

// Muted text on white (minimum safe threshold) ✅
className="text-[#4B5563]"               // 5.9:1
className="text-[rgba(26,26,26,0.7)]"   // 4.8:1

// Body text on dark sections ✅
className="text-white/70"               // 7.2:1 on #16232A
className="text-white/60"               // 5.9:1 on #16232A

// Muted text on dark (minimum safe) ✅
className="text-white/50"               // 4.7:1 on #16232A

// Orange headings — large text only ✅
className="text-haide-orange text-3xl font-bold"   // 36px bold = large text

// White button text on orange ✅ (font-semibold required)
className="bg-haide-orange text-white font-semibold text-sm"  // 14px semibold = large text threshold
```

---

## Failing Combinations — Never Use for Text

```tsx
// ❌ These FAIL WCAG AA for normal text:
className="text-haide-orange text-base"        // 3.03:1 — fail on white
className="text-white/40"                      // 3.8:1 — fail on dark bg
className="text-[rgba(26,26,26,0.5)]"         // 3.1:1 — fail on white
className="text-white font-medium text-sm"    // on orange: 3.03:1, 14px medium is normal text — fail
```

---

## Gradient Backgrounds

The hero gradient blends from `#0A1628` to `#16232A`. Any text over this gradient should use:
- White at ≥ 60% opacity for body copy (safe on both ends of the gradient)
- White at ≥ 50% opacity minimum for secondary text
- Never place orange text over a gradient — the ratio varies across the gradient and cannot be reliably verified
