# Product

## Register

brand

## Users

Pengurus Pasukan (sukan / co-curricular leads) in Malaysian schools — non-engineer teachers who handle SMIS sports event registration. They land on the site after seeing it shared in a teacher Facebook/WhatsApp group, hearing about it from a colleague, or searching for "automasi SMIS / SMIS extension". They are 35-55 years old, time-poor, mid-event-deadline, and skeptical of Western-built SaaS that wasn't designed for their workflow. Bahasa Malaysia is their working language. The deputy principal may glance at the screen while the teacher is browsing.

## Product Purpose

A landing page for SMIS Helper — the Chrome extension that automates SMIS sports event registration. The page exists to convert a curious teacher into someone who:

1. Trusts that a real Malaysian person built this for their exact job (not generic SaaS)
2. Understands the time savings concretely (2 hours → 10 minutes)
3. Sees the freemium promise (15 peserta percuma) without feeling pressured
4. Either installs the free extension from Chrome Web Store, or buys the Pro license (RM59 launch promo / RM99 normal post-promo)

Success looks like a teacher closing the laptop having installed the extension and feeling "this is going to save my life next event." Failure looks like a teacher closing the tab thinking "another one of those SaaS pitches."

## Brand Personality

Three words: **gentle · patient · professional** (温和 · 耐心 · 专业).

Voice: matter-of-fact, BM-first, addresses the teacher's actual workflow pain. Quietly confident, never marketing-shouty. The page should feel like a respected colleague describing a tool they built, not a startup pitching a product.

Emotional target: **relief** — the teacher should leave the page feeling "I've been waiting for this."

## Anti-references

- **Saas startup landing tropes.** Hero with gradient text, three-column features grid with identical icon+heading+text cards, "Trusted by 10,000+ users" social proof when there isn't 10,000, fake testimonials, "Get started free →" generic CTA, sticky exit-intent popup.
- **Aggressive freemium pressure.** Yellow countdown bars, "Only 3 spots left at RM59!", "Save 50%!" overlay, retargeting cookie banners. The promo is real; it doesn't need fake scarcity.
- **English-only marketing copy.** This product is for Malaysian teachers. The page should not read like Google-translated marketing.
- **Generic stock imagery.** Smiling diverse "team" photos, generic laptop-on-desk hero, gradient backgrounds with floating UI mockups. If imagery is used, it should be screenshots of the actual extension or photos of a real Malaysian school context.
- **Glassmorphism / heavy gradients / neon.** Anti-reference for the whole product.
- **Hero metric bragging.** "10x faster!" "Save 95% of your time!" Concrete claims (2 hours → 10 minutes) read as honest; ratios read as marketing.
- **Pricing-comparison tables with checkmarks.** Free vs Pro is a single decision; an elaborate matrix with 12 rows is overkill.

## Design Principles

1. **Honest about what this is.** It's a single-purpose Chrome extension built by one person for Malaysian school teachers. The page should feel like that — specific, narrow, useful — not like a YC pitch deck pretending to be platform-scale.

2. **Show, don't promise.** Real screenshots of the popup and Sedia Dokumen page beat any "Why use SMIS Helper?" bullet list. Concrete time savings beat vague productivity claims.

3. **Patient with skeptical teachers.** A first-time visitor doesn't know what SMIS is automated. Walk them through the workflow they already know — log in to smis.events, fetch peserta, upload Excel, etc. — and show where the extension fits in. Don't assume they understand "extensions" or "Chrome".

4. **Respect the cost decision.** Pricing is upfront, simple, and stays put. No drip-pricing. RM59 launch promo / RM99 normal post-promo. The free tier is a real test, not a teaser.

5. **Brand-grade typography over decoration.** A landing page can be more expressive than the in-app UI; this is brand register, where design IS part of the message. But still no AI-slop tells: no gradient text, no glass cards, no identical-card grids.

## Accessibility & Inclusion

WCAG AA target.

- Contrast ratio ≥ 4.5:1 for body, ≥ 3:1 for UI elements.
- Keyboard navigable (tab through CTA, FAQ, install link, pricing).
- Heading hierarchy semantic (h1 → h2 → h3).
- All meaningful imagery has alt text in BM.
- Bahasa Malaysia primary; secondary English alongside is acceptable but BM should never be afterthought.
- Mobile-friendly (teachers may visit from a phone after seeing the link in a WhatsApp group).
- Respect `prefers-reduced-motion` if any animation is introduced.
