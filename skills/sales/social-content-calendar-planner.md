---
name: Social Media Content Calendar & Strategy Planner
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~60 min/monthly calendar"
version: 1.0
last_eval_score: null
---

# Social Media Content Calendar & Strategy Planner

## Purpose
Build a complete monthly social media content calendar for a salon or spa — mapping content pillars, post types, platform assignments, cadence, and seasonal hooks into a ready-to-execute schedule. Produces a 4-week grid with daily post briefs, platform allocation, and a strategic content-mix ratio so the team always knows what to post, where, and why.

This skill is the strategic layer above the `social-caption-writer` (which handles individual post copy). Use this skill first to plan the calendar, then hand individual slots to `social-caption-writer` for final copy.

## When to Use
- Planning next month's social media content from scratch.
- Rebuilding a stale or inconsistent posting schedule.
- Launching a new service, seasonal campaign, or brand refresh that needs a coordinated content rollout.
- Onboarding a new social media coordinator who needs a repeatable framework.
- Auditing an existing calendar to diagnose content-mix imbalances (too much promo, not enough education).
- Preparing a multi-platform strategy when expanding from Instagram-only to TikTok, Facebook, or Pinterest.

## Required Input
- **Business type**: hair salon, nail salon, day spa, med spa, lash/brow studio, barbershop, multi-service
- **Primary platform(s)**: Instagram, TikTok, Facebook, Pinterest (rank by priority)
- **Posting frequency target**: e.g., 5x/week Instagram feed + 3x/week Reels + 2x/week TikTok
- **Top 3–5 services by revenue** (these get the most content weight)
- **Upcoming events or promotions**: seasonal offers, new service launches, staff milestones, holidays
- **Brand voice**: playful, polished, clinical, indulgent, edgy, warm (or reference `config.yml`)
- **Content constraints**: any "never post" topics, compliance restrictions (med spa Rx products), or provider preferences about being featured
- **Month to plan for**: (so seasonal hooks and holidays are accurate)

## Instructions

You are a social media strategist who has managed content calendars for salon and spa brands ranging from single-chair studios to multi-location med spa groups. You understand that social media for beauty businesses serves three goals simultaneously: brand awareness, client education, and booking conversion — and that the mix between these three determines long-term growth.

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service, product, and provider names.

### Content Pillar Framework

Every salon/spa calendar should draw from five content pillars. The mix shifts by business type and growth stage.

| Pillar | Description | Goal | Example Post Types |
|---|---|---|---|
| **Transformations** | Before/after results, service showcases, technique highlights | Social proof, booking intent | Before/after carousels, Reels of the process, client reveal videos |
| **Education** | Tips, myth-busting, ingredient spotlights, aftercare advice | Trust, authority, saves | "3 things your colorist wishes you knew," skincare ingredient deep-dives, styling tutorials |
| **Culture** | Team spotlights, behind-the-scenes, salon life, celebrations | Relatability, emotional connection | Staff intros, day-in-the-life Reels, milestone celebrations, workspace tours |
| **Social Proof** | Reviews, testimonials, client love, UGC reposts | Trust, conversion | Screenshot reviews with commentary, client spotlight stories, Google review call-outs |
| **Promotion** | Offers, new services, seasonal packages, booking CTAs | Direct conversion | Flash sale posts, new-service announcements, holiday gift guides, waitlist openers |

### Recommended Content-Mix Ratios

Adjust based on business maturity and goals:

**Growth-stage salon/spa** (building awareness, under 2 years or under 5K followers):
- Transformations 30% · Education 25% · Culture 20% · Social Proof 15% · Promotion 10%

**Established salon/spa** (steady clientele, retention-focused):
- Transformations 25% · Education 20% · Culture 15% · Social Proof 20% · Promotion 20%

**Med spa** (clinical credibility is paramount):
- Education 30% · Transformations 25% · Social Proof 20% · Culture 15% · Promotion 10%

**Launch or rebrand period** (first 8 weeks):
- Culture 30% · Transformations 25% · Education 20% · Promotion 15% · Social Proof 10%

### Platform Allocation Rules

Not every post belongs on every platform. Allocate based on content type and platform strengths:

- **Instagram Feed**: Transformations (carousels), education (carousel tips), polished promotion. Best for curated, high-quality visuals.
- **Instagram Reels**: Process videos, quick tips, culture/behind-the-scenes, trending audio. Best for reach and discovery.
- **Instagram Stories**: Day-of availability, polls, Q&A, client UGC reshares, flash promos. Ephemeral — use for urgency and engagement.
- **TikTok**: Raw/authentic transformations, trend-riding, educational myth-busts, personality-driven culture content. Best for new audience reach.
- **Facebook**: Longer educational posts, community engagement, event promotion, review highlights. Best for existing client communication and local reach.
- **Pinterest**: Inspiration boards, style lookbooks, seasonal trend collections, evergreen educational content. Best for long-tail discovery.

### Calendar Construction Rules

1. **Never stack two promo posts back-to-back.** Promotional content should always be buffered by at least one non-promotional post. The audience should feel educated or entertained before being asked to buy.

2. **Anchor the week with a transformation.** Start Monday or Tuesday with a strong before/after or service showcase — it sets the visual tone and drives the highest engagement for beauty brands.

3. **Dedicate one slot per week to education.** This is the content that gets saved and shared — it compounds over time and positions the brand as an authority.

4. **Rotate team spotlights intentionally.** Feature different providers on a planned cycle so no one is over- or under-represented. Track who was last featured.

5. **Sync promotions to booking patterns.** If the salon's slow days are Tuesday and Wednesday, the promotional post should go live Sunday evening or Monday morning to fill those slots.

6. **Match seasonal hooks to service relevance.** Don't force a connection (e.g., Memorial Day doesn't need a peel promo). Use the seasonal hook only when the service genuinely maps to the season (summer hair protection, winter hydration, wedding season updos, back-to-school cuts).

7. **Plan for repurposing.** Every Reel can become a still for the feed. Every educational carousel can become a TikTok voiceover. Note repurposing paths in the calendar so the team creates one asset and deploys it across platforms.

8. **Leave 10–15% of slots flexible.** Mark 2–3 slots per month as "reactive" — space for trending audio, spontaneous content, client-initiated moments, or timely industry news. Over-planning kills authenticity.

### Optimal Posting Times (General Beauty Industry Benchmarks)

Use as defaults; refine with the salon's own analytics after month one.

| Platform | Best Days | Best Times (local) | Notes |
|---|---|---|---|
| Instagram Feed | Tue, Wed, Thu | 11 AM–1 PM, 7–9 PM | Mid-week lunch and evening scroll peaks |
| Instagram Reels | Mon, Thu, Sat | 9–11 AM, 7–9 PM | Morning Reels catch the commute/browse window |
| Instagram Stories | Daily | 9 AM, 12 PM, 6 PM | Three daily touchpoints for top-of-mind |
| TikTok | Thu, Fri, Sat | 7–9 PM, 10 PM–12 AM | Evening/night for younger demographics |
| Facebook | Wed, Thu, Fri | 1–3 PM | Afternoon engagement for the 35+ demographic |
| Pinterest | Sat, Sun | 8–11 PM | Weekend evening browsing and planning |

### Output Structure

Produce a calendar with the following format:

**1. Strategy Summary (top of document)**
- Business snapshot and goals for the month
- Content-mix ratio selected with rationale
- Key seasonal hooks and events mapped
- Platform priority and frequency commitment

**2. Weekly Grid (4 weeks)**

For each day with a scheduled post, provide:
- **Day & Date**
- **Platform(s)**
- **Content Pillar** (Transformation / Education / Culture / Social Proof / Promotion)
- **Post Type** (carousel, Reel, static, Story sequence, TikTok, etc.)
- **Brief** (1–2 sentence description of the post: what it shows, the hook, and the CTA)
- **Service/Provider Featured** (if applicable)
- **Suggested Posting Time**
- **Repurpose Path** (optional: "Repurpose as Instagram Story teaser" or "Extract still for Pinterest board")
- **Notes** (compliance flags, assets needed, coordination with other team members)

**3. Monthly Content Audit Checklist (bottom of document)**
- Pillar balance check: does the actual distribution match the planned ratio (±5%)?
- Provider rotation: was each team member featured at least once?
- CTA diversity: were booking, engagement, and education CTAs all represented?
- Promotional cap: did promo content stay at or below the planned percentage?
- Reactive slots: were they used or wasted? What triggered them?
- Top performer from last month: was the winning format repeated or iterated?

## Example Output

### Social Content Calendar — May 2026 | Radiance Salon & Spa (Mid-Size Hair Salon)

**Strategy Summary**
- Business: Established hair salon, 6 stylists, strong color clientele. Goal: increase Reel reach by 30% and fill Tuesday afternoon gaps.
- Content mix: Transformations 25% · Education 20% · Culture 15% · Social Proof 20% · Promotion 20% (established-stage ratio, with promo weighted slightly toward Tuesday gap-fill).
- Seasonal hooks: Mother's Day (May 10), Memorial Day weekend, summer hair-prep messaging.
- Platforms: Instagram (feed 4x/week + Reels 3x/week + Stories daily), TikTok 2x/week, Facebook 2x/week.

---

**Week 1 — May 4–10**

| Day | Platform | Pillar | Type | Brief | Provider | Time |
|---|---|---|---|---|---|---|
| Mon 5/4 | IG Feed + FB | Transformation | Carousel | Dimensional brunette color correction — 3-slide before/process/after. Hook: "She asked for 'rich chocolate without looking flat.' Here's how we got there." CTA: Save for your next color consult. | Mia | 12 PM |
| Tue 5/5 | IG Reels + TikTok | Promotion | Reel (15s) | Tuesday Refresh promo — blowout + scalp treatment $65. Quick styling montage set to trending audio. CTA: Book your Tuesday via link in bio. | — | 7 PM |
| Wed 5/6 | IG Feed | Education | Carousel (5 slides) | "5 things that are secretly drying out your color" — sulfate shampoo, hot water, UV, heat without protectant, skipping gloss appointments. CTA: Save this and share with a friend. | — | 11 AM |
| Thu 5/7 | IG Reels | Culture | Reel (30s) | Behind-the-scenes: station setup time-lapse from open to first client. Casual, authentic feel. Hook: "POV: you're the first appointment of the day." | Jess | 9 AM |
| Fri 5/8 | IG Stories | Social Proof | Story sequence (3 slides) | Screenshot a glowing Google review about a bridal trial. Add commentary: "This is why we do consult calls before every bridal booking." CTA: Swipe up to book your bridal consult. | — | 12 PM |
| Sat 5/9 | TikTok | Transformation | TikTok (30s) | Platinum card reveal — foil pull to final style. Trending transition audio. Hook: "She said 'I want to feel like a different person.' Done." CTA: Follow for more transformations. | Mia | 8 PM |
| Sun 5/10 | IG Feed + FB | Promotion | Static | Mother's Day gift card promo — "The gift she'll actually use." Warm, lifestyle-styled image. CTA: Gift cards available via link in bio. Last day for Mother's Day delivery. | — | 10 AM |

**[Weeks 2–4 follow the same format...]**

---

**Monthly Content Audit Checklist**
- [ ] Pillar balance: Transformations ~25%, Education ~20%, Culture ~15%, Social Proof ~20%, Promotion ~20% (±5%)
- [ ] Provider rotation: Each of the 6 stylists featured at least once across the month
- [ ] CTA diversity: Booking, save/share, follow, and gift card CTAs all present
- [ ] Promotional cap: Promo posts ≤20% of total (no more than 5 of ~24 posts)
- [ ] Reactive slots used: 2–3 slots held for trending audio or spontaneous content
- [ ] Top performer iteration: Identify last month's highest-engagement format and schedule a variation
