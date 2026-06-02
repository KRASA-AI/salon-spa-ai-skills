---
name: Salon & Spa Loyalty Program Builder
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/program design"
version: 1.1
last_eval_score: 9.0
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

### Config Integration — What To Pull From `config.yml`

Map every value from `config.yml` to a per-key fallback. The program blueprint must ship pre-named to the practice's real stack — no generic "the salon" placeholders.

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Program-name shortlist (often borrows a brand fragment), the enrollment SMS sign-off, the in-chair pitch script attribution, and the welcome-email subject line | "the practice" — flag in the rationale block that the name placeholder must be filled before staff training |
| `business.business_type` | Selects the salon / day-spa / med-spa column of the 2026 industry-reference rebooking-uplift and retail-attach bands; gates the med-spa compliance hook (no-clinical-outcome language on tier descriptions, HIPAA-aware enrollment copy, state refund-grace flag); med-spa loyalty programs **default to retail-and-add-on points only — never points on injectable spend** (FDA/state-board risk of inducement claims). If any `med-spa-*` cadence class exists in `services.cadence_class`, render the med-spa earn-rule guard for those tiers only | default: salon. Flag the assumption inline and offer the hybrid handling if `services.cadence_class` shows mixed cadence classes |
| `business.location.state` | Drives the points-expiration legality flag — most US states allow expiration with disclosure, but **CA Civil Code § 1749.5 prohibits expiration on gift-card-equivalent credit** (the "$50 retail credit" rung is a gift card if denominated in dollars; rename to "complimentary retail set" or scope to points-only to clear); **CO HB 1024** display rule, **IN SB 282** disclosure rule, **NJ February 2026** joint-rule injectable-incentive guard for med-spa tiers, **CA AB-489 (effective 2026-01-01)** restricted-term gate on any reward name that implies licensure ("doctor-level," "clinician-formulated") | use the federal default, flag the missing state explicitly in the Risk Flags section, and apply the conservative CA-style gift-card rule to any dollar-denominated reward — never silently default |
| `services.cadence_class` | Drives the reverse-engineered earn-burn math (the "achievable in 6–8 visits" rule keys off visit frequency by cadence class — `color-cadence` 6-week visits hit the first reward in ~10 weeks, `med-spa-injectable` 12-week visits need a faster earn rate); gates tier-bundling rules (`injectable-cadence` cannot be bundled with non-clinical tiers; `color-cadence` belongs in mid/top tier given chair-time; `lash-fill-cadence` is a natural points-on-rebook anchor); flags the loyalty-on-membership conflict (loyalty earn on a membership-included service is a margin leak — default OFF, per the membership-program-builder integration rule) | use the most-common cadence class in `services.menu`; if no signal, default to 6-week visit frequency and flag the assumption in the rationale |
| `services.menu` + `pricing` | Anchors the burn-rung table to live service prices (the $25 / $50 / $65 / $120 / $280 rungs in the example must be replaced with the practice's actual service prices — never carry the example numbers); drives the bonus-earn-on-new-service trigger to specific services the practice actually offers; surfaces the cadence-class-to-service map for the tier-eligibility rule | use the 2026 industry reference median and mark every rung "industry reference, not your prices" — flag the gap and recommend re-running once `pricing` is populated |
| `pricing.average_ticket` + `pricing.visit_frequency` | Drives the burn-rung achievability math (rewards must clear at 6–8 visits at the practice's actual ticket size); anchors the tier-threshold annual-spend bands ($1,000 / $2,500 are placeholders — pull `average_ticket × annual visit count × tier-percentile` to set real bands keyed to the practice's distribution); powers the Success Metrics targets (member 90-day rebook target is the non-member baseline +20–25 pts, not a flat 75%) | use $85 / 7-week defaults and surface the assumption in the rationale block; the burn-rung table renders with a "placeholder math" header until real pricing is supplied |
| `tools` | Names the booking / POS platform and constrains the points-tracking language to what the platform actually supports — **Mindbody / Boulevard / Zenoti / Vagaro / GlossGenius Pro / Meevo all have native loyalty modules; Square / Fresha need a third-party add-on or a manual tally**; if the platform lacks native loyalty, the launch plan replaces the week-1 "configure points in POS" step with a vendor-selection sprint and pushes launch out 4–6 weeks | recommend the integration audit row in Risk Flags as a hard flag and decline to publish the digital-redemption rung until platform fluency is confirmed |
| `loyalty_program` | If populated (existing program), the skill runs in **migration mode** — preserves member balances, names the conversion ratio (typical: 1 legacy point → 1 new point or 1:2 if rebasing to a finer denomination), drafts the member-comms email explaining the change, and flags any rewards that disappear or change value (this is where most migration goodwill is lost); if empty, run in **launch mode** | run in launch mode and surface "no prior program detected" in the rationale; if the practice mentions a prior program verbally without it being in config, flag the migration risk and offer to re-run once the legacy structure is documented |
| `brand_voice` (or `voice.tone`) | Drives the program-name shortlist tone (warm-indulgent: Radiance Rewards / The Glow Circle / Signature Studio; clinical-precise: Member Benefits / The Wellness Plan / Renew Membership; boutique-elevated: The Circle / The House / The Membership; playful-direct: Loyalty Club / The Punch Card / Pass) and calibrates the enrollment SMS / in-chair pitch register; the "achievable in 6–8 visits" copy register changes — warm-indulgent emphasizes the feel of the reward, clinical-precise emphasizes the math | default to warm-indulgent and surface the assumption inline |
| `compliance.regulations` (med spa only) | Drives the no-clinical-outcome language audit on tier descriptions and reward names (no "younger-looking skin in 5 visits"), the HIPAA-aware enrollment copy check (no clinical service categories in external SMS), the **FDA inducement-claim guard** on any med-spa-injectable points rule (federal — applies in every state), the **CA AB-489** restricted-term gate on reward and tier names, the **NJ February 2026** joint-rule injectable-incentive guard, and the medical-director sign-off line on the launch checklist | omit the med-spa-specific Risk Flags rows; if any `med-spa-*` cadence class exists but `compliance.regulations` is empty, surface a hard flag — do not launch a med-spa-inclusive loyalty program without medical-director sign-off |

If a required config key is missing, produce the program blueprint anyway and surface the gap in the Risk Flags section (extended output) or in the rationale block (standard output) — never silently substitute a fallback as if it were the practice's confirmed input. The program's value depends on transparent assumptions; a launch that ships with unverified burn-rung dollar amounts or an unsupported platform module will burn enrollment goodwill on day one.

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
