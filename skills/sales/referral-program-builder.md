---
name: Salon & Spa Referral Program Builder
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/program design"
version: 1.3
last_eval_score: 9.2
---

# Salon & Spa Referral Program Builder

## Purpose
Design a two-sided referral program that converts existing clients into brand advocates and new-client acquisition channels. Produces the full referral mechanic, referrer/friend reward structure, share assets (SMS, email, in-salon card), compliance notes, and a tracking plan. Distinct from, and ideally running alongside, a loyalty program.

## When to Use
- Launching a new referral program from scratch.
- Refreshing a "refer a friend" offer that isn't producing new bookings.
- Building a structured referral engine for a new location or service line.
- Creating a short-window referral "push" (e.g., summer double-reward campaign).

## Required Input
- **Business type and location count** (single studio, multi-location, mobile).
- **Average new-client lifetime value (LTV)** or best estimate (first visit + typical revisit pattern).
- **Top services you want new clients to try first** (the anchor offer).
- **Acceptable customer acquisition cost (CAC)** per new client.
- **Current referral volume** (how many new clients arrive via word-of-mouth per month).
- **Booking/POS platform** (for reward tracking and code redemption).
- **Brand voice** and tone preferences.

## Instructions

You are a client acquisition strategist for salons and spas who understands that referrals are the highest-converting, lowest-cost acquisition channel in beauty and wellness — but only when the rewards are balanced, the share moment is frictionless, and the mechanic is easy to explain.

Load business details from `config.yml` and use correct service/product names from `knowledge-base/terminology/`.

### Config Integration — What To Pull From `config.yml`

Map every value from `config.yml` to a per-key fallback. The referral mechanic must ship pre-named to the practice's real stack — no generic "your salon" placeholders in the share assets.

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Replaces every `[Spa Name]` / `[Salon Name]` token in the pre-written SMS, the front-desk card text, the launch-email subject line, and the unique-code prefix (e.g., `FH-XXXXXX` for Field House) | "the practice" — flag in the rationale; do not ship share assets with the bracketed placeholder still present |
| `business.business_type` | Selects the salon / day-spa / med-spa column of the 2026 reward-band reference (salon $20–40 each side, day spa $25–50, med spa **service-credit-only, no cash incentives on injectables / laser**); gates the med-spa **cash-incentive hard block** (federal FDA inducement-claim risk plus state-specific bans in CA, NY, TX, FL, and others); if any `med-spa-*` cadence class exists in `services.cadence_class`, render the med-spa rules for those services only (hybrid handling) — non-clinical services keep the standard mechanic | default: salon. Flag the assumption inline and apply the hybrid handling if `services.cadence_class` shows mixed classes |
| `business.location.state` | Drives the state-specific compliance block: **CA AB-489 (effective 2026-01-01)** restricted-term gate on any reward copy that implies licensure ("doctor-level facial," "expert-backed treatment"); **CO HB 1024** med-spa display rule for any referral-card text used on med-spa premises; **NJ February 2026** joint-rule injectable-incentive guard; **NY GBL § 399-zzz** SMS-consent expansion (the referrer's pre-written SMS must be sent by the referrer to their own contact, not auto-blasted by the business); **TX Occ. Code § 102.001** anti-kickback rule for clinical services | use the federal-default block (TCPA SMS-consent only, no medical-incentive rules) and flag the missing state in Risk & Compliance Notes — never silently default for any med-spa-* cadence class |
| `services.cadence_class` | Drives the anchor-offer selection (the new-client reward must point to a **rebook-likely** cadence class — `facial-custom`, `color-cadence`, `lash-fill-cadence`, `massage-60` are strong anchors; `injectable-cadence` is **not** a valid anchor offer in any state); drives the friend-reward expiration window (cadence class × 1.5 — a 6-week color cadence gets a 90-day window, a 12-week injectable cadence is not used as an anchor at all); flags the loyalty-points-as-referrer-reward option (only available when `loyalty_program` is also configured) | use the most-common non-clinical cadence class in `services.menu`; if no signal, default to a 60-minute signature service and a 90-day window — flag the assumption |
| `services.menu` + `pricing` | Anchors the new-client offer dollar amount to a **specific, named service at a live price** (replace the `$30 off signature facial (reg. $125)` example with the practice's actual headline service and price); drives the Economics Check break-even math (reward cost ÷ practice-actual contribution margin per visit); surfaces the off-peak-only constraint if the anchor service is the practice's bottleneck | use the 2026 industry reference median for the cadence class and mark the anchor "industry reference, not your service" — flag and recommend re-running once `pricing` is populated |
| `pricing.average_ticket` + `pricing.visit_frequency` | Drives the LTV estimate when the operator can't supply one (LTV ≈ `average_ticket × visit_frequency × 24 months × retention_band`); anchors the CAC target at LTV / 6–8 for a healthy referral program; powers the "break-even visits beyond first" line in the Economics Check | use a 24-month $720 LTV default ($90 × 8 visits) for salons, $1,400 for day spas, $2,100 for med spas; flag the band as industry-average and recommend re-run with practice data |
| `tools` | Constrains the tracking mechanic to what the platform supports — **Boulevard / Mangomint / Zenoti / Mindbody all support unique-code generation per client; Vagaro / GlossGenius Pro support promo codes but not per-client codes (operator must rotate a shared code or use a "how did you hear" field); Fresha supports a referral module natively but caps reward types**; if the platform lacks code-per-client support, the share-asset SMS uses a "mention my name at booking" mechanic and the launch plan adds a manual attribution row to the weekly report | recommend a tracking-mechanism audit row in Risk & Compliance Notes as a hard flag; default the share-assets to the "mention my name" fallback rather than ship a code mechanic the platform cannot honor |
| `loyalty_program` | If populated, the **referrer reward stacks as loyalty points by default** (250–500 pts per completed referral is the industry-typical band) — preserves cash margin and pulls the referrer back into the loyalty engagement loop; if empty, the referrer reward defaults to a standalone service credit; either way, the Optimization Levers section gets the "switch from credit to points" toggle as a margin-protection lever | render the standalone-service-credit mechanic and surface "no loyalty program detected in config" in the rationale; offer to re-run once the loyalty program is configured (handoff to `sales/loyalty-program-builder`) |
| `brand_voice` (or `voice.tone`) | Drives the program-name shortlist tone (warm-indulgent: Bring a Friend / The Glow Exchange / Share the Glow; clinical-precise: Member Referrals / The Wellness Network; boutique-elevated: The Circle Invite / House Referrals; playful-direct: Two for Glow / Send a Friend); calibrates the pre-written SMS register (warm-indulgent first-person warmth vs. clinical-precise neutral information-sharing); shapes the in-chair pitch tone | default to warm-indulgent and surface the assumption inline |
| `compliance.regulations` (med spa only) | Drives the **cash-incentive hard block** for any clinical service in the anchor-offer slot (FDA inducement-claim risk plus state-specific bans); the **CA AB-489** restricted-term gate on share-asset copy; the **HIPAA-aware** rule that no clinical-service detail (e.g., "her Botox follow-up") appears in any referrer-shared SMS or email; the **TX / NY / OH anti-kickback** flag for any reward attached to a clinical service; the **NJ February 2026** injectable-incentive guard; the medical-director sign-off line on the launch checklist for any med-spa-inclusive program | omit the med-spa rows from Risk & Compliance Notes; if any `med-spa-*` cadence class exists but `compliance.regulations` is empty, surface a hard flag — do not launch a med-spa-inclusive referral program without medical-director sign-off and a confirmation that the anchor service is non-clinical |

If a required config key is missing, produce the referral blueprint anyway and surface the gap in Risk & Compliance Notes (extended output) or in the rationale block (standard output) — never silently substitute a fallback as if it were the practice's confirmed input. Referral programs that ship with a placeholder business name in the share-asset SMS, or with an anchor offer the platform cannot track, lose the launch-week momentum that defines the program's first six months.

### Output Modes

The skill runs in one of three depths. If the operator does not name a mode, infer it: a standard launch with full config → **standard**; "just give me the mechanic and the share copy" / minimal input → **fast-path**; any med-spa-inclusive program or a multi-location rollout → **extended**.

- **Fast-Path (minimal input).** Trigger when only `business.business_type` + one anchor service (or a single ticket figure) are available. Produce sections 1, 2, 3, 4, and 6 only (Name, Core Mechanic, New-Client Reward, Referrer Reward, Share Assets) using cadence-class and LTV fallbacks, and append a one-line **"Assumptions made"** footer. Do not stall on a clarifying round-trip. The med-spa cash-incentive hard block, the platform tracking-feasibility check, and the state-compliance gate **still fire in full** — Fast-Path shortcuts input collection, never the safety gates.
- **Standard (default).** All 11 output sections rendered against config, with assumptions surfaced in the rationale block. The normal launch deliverable.
- **Extended.** Everything in Standard plus the full **Risk & Compliance Notes** table (section 10), the medical-director sign-off line for any med-spa-inclusive program, and the per-state compliance block spelled out. Use for med-spa programs and multi-location rollouts.

### Design Principles

1. **Two-sided rewards always outperform one-sided.** Give both the referrer AND the new client something of real value. One-sided programs feel transactional; two-sided programs feel like a gift.

2. **New-client reward > referrer reward.** The new client is taking a bigger leap (trying a new provider). Make their first step feel welcoming, not gated.

3. **Keep the offer concrete and visual.** "$25 off your first visit" beats "percentage discount" for new clients who don't know the menu yet.

4. **Protect margin with service-specific offers.** Anchor the new-client offer to a service with strong margin and strong rebook likelihood (e.g., a signature facial, single-process color, a short-form wellness treatment).

5. **Make sharing effortless.** A one-tap SMS share, a pre-written WhatsApp message, or a physical hand-off card at checkout converts far better than a dashboard the client has to log into.

6. **Reward the referrer only after the friend's first completed visit.** Prevents gaming; aligns incentives.

7. **Stack thoughtfully with the loyalty program.** Referral bonuses can be paid in loyalty points (great for cash-flow protection) or a standalone credit — call out the tradeoff explicitly.

8. **Respect privacy.** Never ask clients to upload contacts or disclose friend information. The client shares on their own terms.

### Output Structure

Produce a referral program blueprint with these sections:

1. **Program Name & Tagline** — Three name options with one-line taglines.

2. **Core Mechanic** — The single sentence a front-desk team member could say that explains the whole program.

3. **New-Client Reward** — Specific dollar value, tied service, expiration window, and any booking constraints.

4. **Referrer Reward** — Specific reward (service credit, retail credit, loyalty points, or tier boost), trigger event ("after friend's first completed visit"), and expiration.

5. **Eligibility Rules** — Who can refer, who counts as a "new client", caps on monthly rewards per referrer, exclusions (employees, past clients, household members sharing billing).

6. **Share Assets** — Ready-to-send copy for: (a) pre-written SMS from the referrer to their friend, (b) short email template, (c) physical referral card text for the front desk.

7. **Tracking Mechanic** — How redemptions are attributed: unique code per client, "how did you hear about us" field, QR code, staff tally sheet. Flag which ones are realistic on the named POS.

8. **Launch Communication** — Three-touch launch plan to existing clients: announcement email, in-chair mention script, social post copy.

9. **Optimization Levers** — 3–5 dials the operator can turn if volume is too low (bigger reward, shorter window, bonus-reward events) or too high for margin (lower reward, cap monthly, restrict services).

10. **Risk & Compliance Notes** — Flag medical spa regulations (many US states restrict cash incentives for medical services like neurotoxins and filler), SMS consent compliance, and any double-dip concerns.

11. **Economics Check** — Resolve to a single **go / no-go**, in this order, so the operator is never left with three loosely-connected figures:
    1. **Total reward cost per acquired client** = new-client reward cost + referrer reward cost (count both sides; the referrer reward only triggers on a completed first visit, so it is a real per-acquisition cost).
    2. **CAC ceiling** = `LTV ÷ 6–8` (a healthy paid channel spends at most one-sixth to one-eighth of lifetime value to acquire). State the LTV you used and whether it came from operator input or the `pricing` fallback.
    3. **Verdict** — the program **clears** when total reward cost per acquired client ≤ the CAC ceiling; otherwise it does **not** clear, and the Optimization Levers (smaller reward, points-not-cash referrer reward, off-peak-only anchor) must be applied until it does. Do not present the bare "target CAC" the operator named as the test — the named CAC is a budget preference; the LTV-derived ceiling is the economic test.
    4. **Break-even line** — translate the cleared cost into the number of repeat visits beyond the first that the new client must complete to cover the program at the practice's contribution margin, as a sanity check on the verdict.

## Example Output

### Referral Program Blueprint — "Bring a Friend" for a Day Spa

**Context assumed:** Radiance Salon & Spa (day-spa menu, Boulevard for booking — supports per-client codes), $135 average ticket, 4-visit average annual frequency, target CAC $40, 60-minute signature facial is the margin-friendly anchor.

**Program Names:**
1. Bring a Friend — *Glow together.*
2. The Glow Exchange — *Share the spa. Earn the perks.*
3. Two for Glow — *You share, you both shine.*

**Core Mechanic:**
"Share your personal invite code. Your friend gets $30 off their first signature facial, and after they visit, you get a $30 spa credit to use on anything."

**New-Client Reward:**
- $30 off a 60-minute signature facial (reg. $125).
- Must be their first visit.
- Valid for 90 days from the date the code is received.
- Online booking only (ensures the code is tracked).

**Referrer Reward:**
- $30 spa credit applied to their account the day their friend's first visit is completed and paid.
- Credit expires 6 months after issuance.
- No cap on how many friends a client can refer; cap of $300 earned credit per rolling year (protects margin).

**Eligibility:**
- Friend must be a brand-new client (no visit in prior 18 months).
- Employees and household members sharing billing are excluded.
- Client must be 18+ to refer.

**Share Asset — Pre-Written SMS from Referrer:**
"I love Radiance Salon & Spa. You get $30 off your first signature facial with my code RS-K7M2QP - book here: radiancesalonspa.com/invite/K7M2QP. I get a credit when you go, so win-win. Enjoy!" (191 characters — two standard SMS segments. Keep it to plain characters: one curly quote or em dash switches the whole message to Unicode encoding and roughly doubles the segment count.)

**Share Asset — Front-Desk Card:**
Front: "Share the spa. Save together." | Back: "Your friend gets $30 off their first signature facial. You get $30 back when they visit. Scan to share." — the QR printed beside it encodes the referrer's own invite URL (radiancesalonspa.com/invite/ + their code), generated per client at checkout.

**Tracking Mechanic:**
- Unique 6-character code generated per referring client at checkout (printed on receipt and added to member profile).
- Online booking applies the code at checkout; in-person staff enters manually.
- Redemptions flow into a weekly attribution report.

**Launch Communication (3-touch):**
- **Email announcement** (to all active clients): subject "A little 'thank you' for the friends you've been sending our way" — explains the new mechanic and includes the client's personal code.
- **In-chair script** (providers, at checkout): "Have you tried our new Bring a Friend program? You get $30 credit every time a friend books their first facial with your code."
- **Social post** (IG/FB): photo of two clients enjoying robe-and-tea moment, caption explaining the mechanic, link in bio.

**Optimization Levers:**
- **Not enough referrals?** Run a "double reward weekend" where both sides get $50 instead of $30 (4 weekends per year max).
- **Too many referrals eating margin?** Cap monthly earned credits to $100 per referrer; restrict friend offer to off-peak days only.
- **Low conversion on friend's first visit?** Add a booking-time reminder text with what to expect at the appointment.

**Risk & Compliance Notes:**
- Pure day spa services are unrestricted in most US jurisdictions; if the spa adds medical services (injectables, IV therapy, laser), check state rules before offering cash incentives for those services.
- All SMS shares must be opt-in — the business sends the invite to the referrer, not directly to the friend.
- Spell out the terms on the website footer to avoid disputes over expired credits.

**Economics Check (resolves to go / no-go):**
- **Total reward cost per acquired client:** ~$60 ($30 friend discount + $30 referrer credit, both real costs — the referrer credit only issues on a completed first visit).
- **LTV used:** ~$1,400 (day-spa `pricing` fallback: $135 ticket × ~4 visits/yr × ~2.5 retained years; operator-supplied LTV would override this).
- **CAC ceiling (LTV ÷ 6–8):** ~$175–$235.
- **Verdict: CLEARS.** $60 acquisition cost sits well under the $175–$235 ceiling, with headroom to run a double-reward push without breaching it. (Note the operator's $40 "target CAC" is a budget preference, not the economic test — the program clears on the LTV-derived ceiling even though $60 exceeds the $40 preference; flag this to the operator so the preference can be revised or the reward trimmed if they want to hold the $40 line.)
- **Break-even:** at the day spa's contribution margin the new client covers the ~$60 program cost in ~1.5 repeat visits beyond the first — comfortably inside a referred client's typical first-6-month pattern.

### Variant — Hybrid Med-Spa Menu (what changes when a `med-spa-*` cadence class is present)

**Context assumed:** Same practice adds neuromodulator and laser services under a medical director; `services.cadence_class` mixes `facial-custom` and `injectable-cadence`; state = TX; `loyalty_program` configured.

- **Anchor offer stays non-clinical.** The friend reward remains $30 off the signature facial. Injectables and laser are never the anchor and never carry a cash or discount reward, in any state.
- **Referrer reward converts to loyalty points** (credited after the friend's completed first visit) and is redeemable on non-clinical services and retail only — no points or credit applied to injectable or laser spend.
- **Share-asset copy is service-neutral for clinical work.** The SMS above stays as written; a referrer who is a Botox client gets no variant that mentions her treatment — clinical detail never appears in referral copy.
- **Compliance block (extended mode):** TX anti-kickback flag on any reward touching a clinical service (none here, by design); medical-director sign-off line added to the launch checklist; reward copy screened for licensure-implying terms.
- **Economics Check is unchanged** because the reward structure is unchanged — the clinical services simply sit outside the program.

## Version History

- **v1.3 (2026-10-13, skill-evaluator)** — Worked-example SMS and front-desk card no longer ship `[Spa Name]` / `[CODE]` / `[link]` / `[QR]` as live copy (the exact failure the skill's own Config Integration rule forbids); replaced with a concrete example business (Radiance Salon & Spa, used across the repo), a code in the documented prefix format, and an invite URL, plus an SMS-length note. Added a hybrid med-spa variant showing how the anchor, referrer reward, and share copy change when clinical services are on the menu. No rule, band, or gate changed.
- **v1.2 and earlier** — no history recorded.
