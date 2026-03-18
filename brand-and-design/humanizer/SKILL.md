---
name: humanizer
description: >
  Detect and remove AI writing patterns from social media content, captions,
  threads, carousels, and any written copy. Use whenever writing or reviewing
  Final Copy, AI Drafts, or any content before it goes to Notion or Postiz.
  Trigger phrases: "humanize this", "check for AI patterns", "run the humanizer",
  "does this sound human", "review this copy".
---

# Humanizer

Detect and eliminate AI writing patterns from content. Every piece of written
copy — captions, threads, carousels, text posts — must pass this check before
being considered done.

## Process

1. Scan for banned patterns (below) — flag every instance
2. Rewrite flagged sections — don't just delete, replace with direct human language
3. Score with the rubric — must hit 35/50 to pass
4. If below 35, revise and rescore

---

## Banned Patterns

### 1. Throat-Clearing Openers

Cut these entirely. State the point directly.

- "Here's the thing:"
- "Here's what [X]" / "Here's why [X]" / "Here's how [X]"
- "The uncomfortable truth is"
- "It turns out"
- "The real [X] is"
- "Let me be clear"
- "Let's be honest"
- "The truth is,"
- "I'll say it again:"
- "I'm going to be honest"
- "Can we talk about"
- "Here's what I find interesting"
- "Here's the problem though"
- Any "Here's [anything]" construction — it's always throat-clearing before the point

### 2. False Contrasts

State the point directly without the negation setup.

- "X isn't Y. It's Z." — the classic false contrast pattern
- "It's not about X. It's about Y."
- "This isn't X. This is Y."
- Negative listing: describing what something *isn't* before revealing what it *is*
- "Not just X, but Y. Not just A, but B." — excessive parallelism

### 3. Emphasis Crutches

These add zero meaning. Delete them.

- "Full stop." / "Period." (used as a rhetorical finisher)
- "Let that sink in."
- "This matters because"
- "Make no mistake"
- "Here's why that matters"
- "And that changes everything."

### 4. Adverbs

Kill all adverbs. No -ly words. They weaken the statement they modify.

Specific offenders: really, just, literally, genuinely, honestly, simply,
actually, deeply, truly, fundamentally, inherently, inevitably, interestingly,
importantly, crucially, essentially, ultimately, basically, clearly, obviously

Exception: "honestly" and "I think" are acceptable in Adrian's personal voice
when expressing genuine uncertainty — not as filler.

### 5. Business Jargon

| Avoid | Use instead |
|-------|-------------|
| Navigate (challenges) | Handle, address, deal with |
| Unpack | Explain, examine, break down |
| Lean into | Accept, embrace, commit to |
| Landscape | Situation, field, market |
| Game-changer | Significant, important |
| Double down | Commit, increase |
| Deep dive | Analysis, examination |
| Take a step back | Reconsider |
| Moving forward | Next, from now |
| Circle back | Return to, revisit |
| On the same page | Aligned, agreed |
| Unlock | Enable, open up |
| Drive results | Produce results, achieve |
| Empower | Let, allow, give |
| Leverage | Use |
| Synergy | (delete entirely) |

### 6. Filler Phrases

Remove — they add no information.

- "In today's fast-paced world"
- "In the ever-evolving landscape of"
- "As we navigate the complexities"
- "In an increasingly digital world"
- "In a world where"
- "At its core"
- "It's worth noting"
- "It's worth mentioning"
- "At the end of the day"
- "When it comes to"
- "The reality is"
- "The fact is"
- "Needless to say"
- "It goes without saying"
- "Imagine a world where..."

### 7. Vague Declaratives

Sentences that announce importance without naming the specific thing.

- "The reasons are structural"
- "The implications are significant"
- "This is the deepest problem"
- "The stakes are high"
- "The consequences are real"
- "This is what leadership actually looks like"
- "This is genuinely hard"
- "That's a big deal"
- "This changes everything"

Replace with the specific thing, or cut.

### 8. Promotional Superlatives

- "groundbreaking", "cutting-edge", "transformative", "revolutionary"
- "world-class", "best-in-class", "industry-leading"
- "unprecedented", "game-changing"
- "powerful" (when used without specifics)

Replace with the specific claim that earns the label.

### 9. Vague Attributions

- "experts say" (without naming who)
- "studies show" (without citing which)
- "research suggests" (without a source)
- "many people think"
- "some would argue"

Either name the source or reframe as a direct claim.

### 10. Meta-Commentary

Remove self-referential announcements. The content should move, not narrate itself.

- "Let me walk you through..."
- "In this post, I'll..."
- "I want to explore..."
- "As we'll see..."
- "Plot twist:" / "Spoiler:"
- "You already know this, but"
- "Hint:"
- "But that's another post"
- "X is a feature, not a bug"

### 11. False Agency

Don't give inanimate objects human actions.

- "The data tells us..." → "The data shows..." or name who's interpreting it
- "The culture shifts..." → name who's shifting it
- "The market decides..." → name the actual mechanism
- "The algorithm rewards..." → "Google's algorithm ranks X higher when..."

### 12. Passive Voice

Restructure to reveal who's doing what.

- "Results were achieved" → "We achieved X results"
- "It was found that" → "We found" / "Research found"
- "Content is created" → "We create content"

Exception: passive is fine when the actor genuinely doesn't matter.

### 13. Rhetorical Setups

Make the point. Let readers conclude.

- "What if I told you..." — cut, state it
- "Think about it:" — cut, state it
- "Ask yourself:" — cut, state it
- "Sound familiar?" — cut
- Questions answered immediately ("What's the biggest mistake? Not doing X.") — merge into a statement

### 14. Dramatic Fragmentation

Sentence fragments used for artificial emphasis feel manufactured.

- "Simple as that."
- "That's it."
- "Works every time."
- Multi-fragment stacks: "One word. Consistency. That's it." — rewrite as a full sentence

### 15. Weak Sentence Starters

- Starting with "So," or "Look,"
- Starting with Wh- words for dramatic effect: "What this means is...", "Why does this matter?"
- "And the reason is..." (just say the reason)

### 16. Rule of Three Overuse

Max one three-item list per post. Using three-item lists in every paragraph is
an AI pattern — vary to two items, four items, or full sentences.

### 17. Em Dash Overuse

Max one em dash per post. Replace extras with periods or restructure the sentence.

---

## Quality Scoring Rubric

Score each dimension 1–10. Must hit **35/50** to pass. Below 35 = revise and rescore.

| Dimension | What to evaluate | Score |
|-----------|-----------------|-------|
| **Directness** | Does it state or announce? Direct = states. Announces = "Here's what..." setups | /10 |
| **Rhythm** | Is sentence length varied or metronomic? Human writing has irregular rhythm | /10 |
| **Trust** | Does it respect reader intelligence? No hand-holding, no "let that sink in" | /10 |
| **Authenticity** | Does it sound like a person wrote it, not assembled it? | /10 |
| **Density** | Can anything be cut without losing meaning? Tighter = better | /10 |
| **Total** | | /50 |

---

## Platform Voice Rules

Always check voice after humanizing:

| Platform | Voice | Check |
|----------|-------|-------|
| LinkedIn Personal | "I" — personal, opinionated | No "we" as subject |
| Threads | "I" — conversational, hot takes | No "we" as subject |
| LinkedIn Newsletter | "I" — analytical, long-form | No "we" as subject |
| X/Twitter | "we" — company, technical | No "I" as subject |
| Instagram | "we" — company, visual | No "I" as subject |
| Facebook | "we" — company, educational | No "I" as subject |
| LinkedIn Company | "we" — company, professional | No "I" as subject |

---

## Adrian's Voice (Personal Profiles)

- Direct, no fluff
- Admits uncertainty: "I think," "not sure yet but...", "roughly"
- Casual imprecision when natural: "pretty much," "kind of"
- Technical but accessible — explains the architecture AND the why
- Confident but not cocky
- Has opinions — "X is better because...", "This approach is wrong and here's why"
- Tells stories from experience — actual client situations, actual test results

---

## Language: US English

All content must use US English spelling: optimize, recognize, organize, analyze,
color, behavior, labor, center, defense. Z over S, -or over -our, -er over -re.
Flag any British spelling (optimise, recognise, colour, behaviour, centre, defence).

---

## Quick Checklist (pre-publish)

- [ ] No "Here's [anything]:" openers
- [ ] No "X isn't Y. It's Z." contrasts
- [ ] No adverbs (-ly words)
- [ ] No vague attributions without sources
- [ ] No promotional superlatives without specific claims
- [ ] No filler phrases ("at its core", "in today's world")
- [ ] No passive voice obscuring the actor
- [ ] No meta-commentary ("let me walk you through")
- [ ] Voice matches platform (I vs we)
- [ ] Em dashes ≤ 1 per post
- [ ] Rule of three used ≤ once per post
- [ ] Score ≥ 35/50 on rubric
