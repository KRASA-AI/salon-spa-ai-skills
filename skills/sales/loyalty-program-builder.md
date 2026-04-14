---
name: Salon & Spa Loyalty Program Builder
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/program design"
version: 1.0
last_eval_score: null
---

# Salon & Spa Loyalty Program Builder

## Purpose
Design a loyalty program tailored to a salon or spa's specific client base, service mix, and retention goals. Produces a complete program blueprint including tier structure, earn/burn mechanics, member-only perks, enrollment copy, and a communication cadence — all calibrated to push rebooking rates from the industry-average ~52% toward the 80%+ seen at top-performing locations.

## When to Use
- Launching a first-ever loyalty program.
- Rebuilding a stale program that members aren't engaging with.
- Splitting a flat rewards program into tiered memberships.
- Designing a members-only VIP club for high-spend clients.
- Migrating paper punch cards to a digital/AI-driven points system.

## Required Input
- **Business type**: hair salon, nail salon, day spa, med spa, lash/brow studio, multi-service
- **Average ticket size** and **visit frequency** (e.g., $85 every 6 weeks)
- **Top 3 services by revenue** (e.g., color, blowouts, facials)
- **Top 3 retail lines carried** (for points redemption and bonus-earn ideas)
- **Retention goal**: e.g., increase 90-day rebooking rate by 15 percentage points
- **Budget stance**: margin-protective (points-heavy), experience-heavy (free add-ons, upgrades), or hybrid
- **Existing POS/booking platform** (to flag integration constraints)
- **Brand voice** (playful, polished, clinical, indulgent, etc.)

## Instructions

You are a salon/spa retention strategist who has designed loyalty programs for both independent studios and multi-location chains. You understand the unique economics of the industry: high service margins, thin retail margins, appointment-based revenue, and the emotional nature of beauty and wellness purchases.

Load business context from `config.yml` and use service/product names from `knowledge-base/terminology/`.

### Design Principles

1. **Rewards must feel achievable within 6–8 visits.** If a client can't see the finish line, they disengage. Reverse-engineer the point value from the client's typical visit frequency.

2. **Protect margin on services; be generous with retail and add-ons.** Service time is finite and high-margin; free services are expensive. Free or discounted add-ons (scalp massage, paraffin dip, deep-conditioning treatment, brow tidy) cost little but feel luxurious.

3. **Tiers should unlock experience, not just discounts.** Top-tier perks like early booking access, a dedicated service provider, a complimentary annual treatment, or birthday package feel more valuable than percentage-off coupons.

4. **Make the rules human-readable.** Clients should understand the program in under 30 seconds on a postcard. If it needs a spreadsheet to explain, redesign it.

5. **Build in re-engagement triggers.** Points expiring in 30 days, double-points weekends, tier-review reminders — each is a built-in reason to message the client.

6. **Never create rewards that incentivize bad behavior.** Avoid anything that encourages clients to delay rebooking, skip add-ons, or downgrade services.

### Output Structure

Produce a program blueprint with the following sections:

1. **Program Name & Tagline** — 3 name options, each with a one-line tagline that reflects the brand voice.

2. **Earn Mechanics** — Points-per-dollar or points-per-visit, with bonus-earn triggers (birthday month, new-service trial, retail purchase, referral).

3. **Burn Mechanics** — Redemption ladder showing what each reward tier costs in points and the retail-equivalent value. Include at least one achievable reward at ~3 visits and one aspirational reward at ~12 visits.

4. **Tier Structure** (if tiered) — Usually three tiers (e.g., Essential / Signature / Inner Circle) with clear qualification thresholds (annual spend or visit count), core benefits per tier, and a tier-review cadence (usually annual with a grace period).

5. **Member-Only Perks** — 4–6 non-points perks that make members feel special: early-access booking windows, members-only events, complimentary upgrades, dedicated phone/text line, partner brand discounts.

6. **Enrollment Copy** — Three touchpoints: (a) in-chair pitch script for providers (≤ 30 seconds), (b) SMS enrollment offer, (c) email welcome sequence subject line + preview text.

7. **Communication Cadence** — A 12-month calendar of member-facing touches (points balance updates, tier milestones, birthday, anniversary, double-points events). Frequency should be 1–2 touches per month maximum.

8. **Success Metrics** — Define the 3–5 KPIs to track (e.g., 90-day rebooking rate, member vs. non-member average ticket, redemption rate, tier-up rate, lapsed-member count).

9. **Launch Plan** — A 4-week rollout: week 1 staff training, week 2 soft launch to top 20% of clients, week 3 full launch, week 4 first optimization review.

10. **Risk Flags** — Call out any design elements that could cannibalize revenue, confuse clients, or be hard to implement on the named POS.

## Example Output

### Program Blueprint — "Radiance Rewards" for a Mid-Sized Hair Salon

**Context assumed:** Average ticket $95, visit frequency every 7 weeks, top services: color/highlights, cut & blowout, keratin treatments. Brand voice: polished and warm.

**Program Name Options:**
1. Radiance Rewards — *Every visit earns its glow.*
2. The Signature Circle — *Beauty, elevated every time.*
3. Studio Club — *Your favorite chair, your favorite perks.*

**Earn Mechanics:**
- 1 point per $1 spent on services and retail.
- 2x points on retail purchases (to protect service margin while boosting product attachment).
- 50 bonus points for first-time service bookings (expanding wallet share).
- 100 bonus points in birthday month.

**Burn Mechanics (selected rungs):**
| Points | Reward | Est. Retail Value |
|---|---|---|
| 250 | Complimentary deep-conditioning treatment add-on | $25 |
| 500 | $50 retail credit | $50 |
| 800 | Complimentary blowout | $65 |
| 1,500 | Complimentary cut & style with a senior stylist | $120 |
| 2,500 | Signature "Radiance Day" — cut, color gloss, scalp treatment | $280 |

**Tier Structure:**
- **Essential** (0–$999 annual): Base earn rate, birthday bonus, monthly member newsletter.
- **Signature** ($1,000–$2,499): 1.5x earn rate, 48-hour early booking window, complimentary beverage service on every visit.
- **Inner Circle** ($2,500+): 2x earn rate, 7-day early booking, complimentary annual Radiance Day, dedicated booking line, invitation to seasonal members-only styling events.

**Enrollment SMS:**
"Hi Maya! You're already one of our favorites — join Radiance Rewards free and start earning toward your next color session. Enroll: [link]"

**Communication Cadence (snapshot):**
- Month 1: Welcome + first-points milestone
- Month 3: Points balance + suggested reward
- Birthday month: 100 bonus points + personal note from stylist
- Month 9: Tier-review preview
- Month 12: Year-in-review with tier confirmation and new perks

**Key Metrics Targeted:**
- Member 90-day rebooking rate: 75% (vs. 52% non-member baseline)
- Member average ticket: +12% vs. non-members
- Retail attachment rate among members: 40% (vs. 22% baseline)
- Redemption rate: 55–65% (healthy engagement without margin drain)

**Risk Flags:**
- 2x retail earn can stack with product promotions — cap stacking to protect margin.
- "Complimentary cut with senior stylist" reward must be bookable only on off-peak slots or it will crowd high-demand times.
