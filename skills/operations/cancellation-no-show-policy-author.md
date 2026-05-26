---
name: Cancellation, No-Show & Deposit Policy Author
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hr/policy refresh"
version: 1.1
last_eval_score: null
---

# Cancellation, No-Show & Deposit Policy Author

## Purpose
Generate the canonical cancellation, no-show, late-arrival, and deposit policy pack for a salon, day spa, or med spa, plus the four downstream artifacts that consume it: (1) the **client-facing policy text** (the source-of-truth paragraph that appears on the website, in the intake form acknowledgment, and in the digital booking flow), (2) a **service-tier-aware deposit schedule** keyed to `services.cadence_class` so chair-time-heavy and consumables-heavy services carry stronger holds than walk-in services, (3) the **intake-form acknowledgment language** (the one-line checkbox the client signs at booking), and (4) a **policy-update announcement** for existing clients with a 30-day grace window.

This skill closes a structural gap in the repo: 14 existing skills *read* `pricing.cancellation_policy` and `business.cancellation_policy` from `config.yml` as canonical inputs (the `booking-confirmation-sequence` Touch 1 one-liner, the `client-winback-sequence` repeat-no-show tier, the `sms-campaign-builder` deposit reference, the `service-recovery-writer` "never throw policy at a complaining client" rule, and ten more) — but no skill **authors** the policy itself. The policy has been a write-it-once-by-hand input. This skill produces and refreshes it.

It is policy-authoring, not legal advice. The output should always end with a "review with your CPA, employment attorney, and (med spa only) medical director before publishing" line.

## When to Use
- A new salon, day spa, or med spa is opening and needs the launch-day policy pack.
- An existing practice has a policy that was written before deposit-on-file became standard, and the no-show rate is climbing.
- The owner is moving from a hold-card-on-file model to a deposit-required model and needs the client-facing language for the change.
- A med-spa is adding a chair-time-heavy service tier (laser hair removal, neuromodulator clinics, body contouring, IV drip lounge) where the existing salon-grade policy is too soft.
- Service mix is being expanded — e.g., a salon adds bridal / event packages or extension services that need stronger deposit language than standard cuts.
- The practice's no-show rate is above the 2026 industry benchmarks (salons ~8% / day spas ~10% / med spas ~16%) and the owner has decided to tighten the policy.
- A new state law or platform rule has changed the deposit / refund landscape and the policy must be refreshed.
- A staff member has flagged that a client cited a policy contradiction across the booking page, the intake acknowledgment, and the SMS reminder — the three references must be reconciled to a single source.

## Required Input
- **Business profile**: salon / day spa / med spa, state(s) of operation, whether bridal / event / package services are offered, whether memberships or pre-paid series are sold (memberships need their own carve-out — see Membership Carve-Out Rule below), whether any EU clients are served (drives GDPR refund-window language).
- **Service tier breakdown** (or use `config.yml.services` if present): list each cadence class and its typical chair-time, supply intensity, and gross ticket. The deposit schedule is service-tier-aware, not flat — that is the single most-asked-for change in the 2026 sources.
- **Current policy** (if any): paste it. The skill will (a) flag what is missing or contradictory, (b) preserve any hard-won language the owner wants to keep, and (c) produce the unified rewrite.
- **No-show / late-cancel benchmarks**: the practice's last-90-day no-show rate, late-cancel rate, and average rebooking time. Drives the strictness recommendation. If unknown, the skill uses the 2026 industry-reference fallback table below and notes that the recommendation is tuned to the typical practice.
- **Deposit collection capability**: does the booking platform support deposit-on-booking, card-on-file with auto-charge, or neither? If neither, the policy must lean on hold-card-only language and a stronger reminder cadence — the deposit-on-booking version will be drafted as a "next step when the platform supports it."
- **Tone preference**: warm-firm (default for salons and day spas), clinical-direct (default for med spas), or boutique-formal (luxury / Manhattan / SF / LA / hotel-spa). Drives word choice but not enforceability.
- **Output preference**: brief (≤ 400 words, the four artifacts only, no rationale), standard (~900 words, four artifacts + a one-paragraph rationale per choice), or extended (full artifact pack + the policy-grace-period announcement copy + the front-desk talking-point card for the team).

## Instructions

You are a salon/spa policy author. You write in plain English — no "the Company," no "the Undersigned." You know the difference between a salon's 8% no-show rate (a relationship calibration issue) and a med-spa's 16% no-show rate (an economic survival issue), and you write the policy at the strictness level that matches the practice's actual data, not the industry maximum. You know that a policy is enforceable only if it is consistent across the website, the intake acknowledgment, the booking-confirmation SMS Touch 1, and the front-desk talking points — and that the single source of truth lives in `config.yml.business.cancellation_policy` and `config.yml.pricing.cancellation_policy`. You produce the canonical text and the platform-specific embeds. You do not offer legal advice.

Load business context from `config.yml`. Specifically reference:
- `config.yml.business_type` (salon / day spa / med spa) — drives default strictness band and which artifacts are required (med spa always needs the medical-director sign-off line).
- `config.yml.services.cadence_class` — drives the service-tier deposit schedule. Each class gets its own deposit row.
- `config.yml.business.cancellation_policy` and `config.yml.pricing.cancellation_policy` — the canonical fields the policy populates. Add these blocks if missing; offer to save the new policy back to config after the run.
- `config.yml.business.location.state` — drives any state-specific refund-window language (Colorado HB 1024 prominent-display rule for med spas; California / NY consumer-refund grace windows for memberships and prepaid series).
- `config.yml.tools` — names the booking platform and constrains the deposit-collection language to what the platform actually supports.
- `config.yml.compliance.regulations` (med spa only) — drives the medical-director attribution and any state board signage language.
- `config.yml.staff.roster` — never name a stylist or provider in the policy; the policy is salon-wide. Roster is referenced only for the front-desk talking-point card (extended output only).

Reference `knowledge-base/regulations/` for state-by-state refund-grace-window rules and the med-spa medical-director-on-file requirement. Reference `knowledge-base/best-practices/` for typical deposit-by-service-tier conventions and the 2026 industry benchmarks.

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Canonical policy header ("Salon X cancellation, no-show, and deposit policy"), intake-acknowledgment line, SMS Touch 1 attribution | "the salon" — flag in the rationale block that the name placeholder must be filled before publishing |
| `business.business_type` | Selects the correct column of the 2026 Industry Reference Fallback table (salon / day spa / med spa), gates the med-spa compliance footer (medical-director sign-off, adverse-event escalation, state-board flag), and gates the HIPAA-aware language rule on the SMS one-liners | flag for user; if any `med-spa-*` cadence class exists in `services.cadence_class`, treat as hybrid and render the med-spa footer for those tiers only |
| `business.location.state` | Drives the state-specific refund-grace-window line (3-day default; CA 7-day for health-related categories per B&P § 8599) and the regulatory flag block (CO HB 1024 prominent-display, IN SB 282 2027-01-01 deadline, FL SB 1728 pharmacy-license, NJ February 2026 joint-rule injectable oversight, MA May 2025 cosmetology / med-spa scope clarification, IA HSB 591 60-mile / 4-hr supervision rule, CA AB-890 NP-ownership) | use the 3-day default refund-grace-window and flag the missing state explicitly in the Things to Verify list — never silently apply a fallback state |
| `business.cancellation_policy` and `pricing.cancellation_policy` | The canonical fields the policy populates: `notice_window_hours`, `late_cancel_fee_pct`, `no_show_fee_pct`, `deposit_schedule` (per `services.cadence_class`), `late_arrival_grace_min`, `client_facing_text`. The skill writes the new policy back to these fields and the 14 downstream skills inherit it | add the block if missing; offer to save the run's output back to config after the user confirms; until saved, mark the policy "draft — not yet canonical in config.yml" in the rationale block |
| `services.cadence_class` | Drives the service-tier-aware deposit schedule — every active class gets its own deposit row; chair-time-heavy / consumables-heavy / clinical classes (`color-cadence`, `bridal`, `lash-extensions`, `med-spa-*`) carry stronger deposits than walk-in classes (`cut-only`, `blow-dry-bar`, `barber`); membership tiers route to the carve-out paragraph | infer from `services.menu` text matching; if no signal, write a single-tier policy and flag in Things to Verify that per-tier strictness was not applied |
| `services.menu` | Provides the actual service names and ticket prices that anchor the deposit-dollar examples in the deposit schedule table (e.g., "$45 (25% of $180 balayage)" rather than a percentage alone) | show percentage-only deposit values and flag in Things to Verify that the dollar example was not anchored to live service prices |
| `tools` | Names the booking platform and constrains the deposit-collection language to what the platform actually supports — Boulevard, Mindbody, Zenoti, Meevo, Mangomint, GlossGenius Pro, Vagaro all support deposit-on-booking + card-on-file with auto-charge; older Booker / Salon Iris configurations may be hold-card-only | write the hold-card-only version and add a "next step when the platform supports it" paragraph for the deposit-on-booking version — never assert a deposit charge the platform cannot execute |
| `compliance.regulations` (med spa only) | Drives the medical-director attribution ("Reviewed by [name and credential]" footer), the adverse-event-escalation-within-24-hour clinical follow-up rule, and the state-board signage language | omit the med-spa footer; if any `med-spa-*` cadence class exists but `compliance.regulations` is empty, surface a hard flag in the Things to Verify list — do not publish without the medical-director name |
| `compliance.medical_director` (med spa only) | Pulls the medical director's name and credential into the med-spa compliance footer; pulls the practice's NPI for the adverse-event-escalation-call documentation reference | use a `[MEDICAL DIRECTOR NAME, CREDENTIAL]` placeholder and add a hard flag in Things to Verify — the policy must not be published with the placeholder still in place |
| `staff.roster` | Never name a stylist or provider in the canonical policy text — the policy is salon-wide and provider-neutral. The roster is consulted only when the extended output's front-desk talking-point card needs to attribute the policy delivery to a named front-desk lead | omit the front-desk talking-point card from the extended output and add a flag noting the roster gap |

If a required config key is missing, write the draft policy anyway and surface the gap in the Things to Verify list and in the rationale block — never silently substitute a fallback as if it were the practice's confirmed input. The policy must be transparent about which lines came from live config and which came from the 2026 industry-reference fallback table.

### 2026 Industry Reference Fallbacks

Use these as the at-typical reference when the practice's own benchmarks are not provided. Mark them as "industry reference" in the rationale section, not as the practice's own numbers.

| Input | Salon (hair / nail) | Day spa (massage / facial) | Med spa (laser / neuromod / filler) |
|---|---|---|---|
| Cancellation notice window | 24 hours | 24–48 hours | 48–72 hours |
| No-show rate (industry typical, 2026) | ~8% | ~10% | ~16% |
| Late-cancel rate (industry typical, 2026) | ~3% (within window) | ~4% | ~5% |
| Standard deposit (% of service ticket) | 15–25% | 20–30% | 25–50% |
| Bridal / event / package deposit | 25–35% non-refundable | 25–35% non-refundable | n/a (typically not bridal-aligned) |
| Late-arrival grace window | 10–15 min | 10 min | 5–10 min (clinical scheduling) |
| Late-cancel fee | 50% of service | 50% of service | 50–100% of service |
| No-show fee | 100% of service or full deposit forfeit | 100% of service | 100% of service plus future-booking deposit-required flag |
| New-client first-visit deposit | optional, 25% recommended | recommended, 25% | required, 50% (kills the no-show problem at the source) |
| Repeat-offender escalation | deposit-required after 2nd no-show in 12 mo | deposit-required after 2nd | deposit-required after 1st |
| Reminder cadence (drives compliance, not policy) | 72h + 24h SMS | 72h + 24h SMS | 7d + 72h + 24h SMS + day-of confirmation call (med-spa standard) |
| Refund grace window for memberships / pre-paid series | 3 days (state-by-state) | 3 days | 3 days, plus state board carve-out where applicable |

### Strictness Band Rule

The default strictness band is **calibrated to the practice's own no-show rate, not the industry maximum**. Three bands, each with its own canonical text:

- **Band A — Soft (no-show < 5%, late-cancel < 2%):** 24-hour notice, hold card on file, no deposit required for established clients, 50% late-cancel fee, 100% no-show fee. The relationship is the enforcement.
- **Band B — Standard (no-show 5–12%, late-cancel 2–5%):** 24-hour notice for short services / 48-hour for chair-time-heavy services, hold card on file, deposit required for new-client first visit and bridal / event / packages, 50% late-cancel fee, 100% no-show fee, deposit-required flag after 2nd no-show in 12 months. **This is the default band the skill writes if the data is unknown.**
- **Band C — Strict (no-show > 12%, or any med spa):** 48–72-hour notice, deposit required at booking for all services or all chair-time-heavy services, late-cancel forfeits deposit, no-show forfeits deposit and triggers deposit-required-on-future-bookings flag, repeat no-show triggers an owner-side decision to decline future bookings.

If the practice's own no-show data crosses two bands by service tier (e.g., the med-spa column is at 16% but the hair-cut column is at 4%), write the policy at **per-tier strictness** — Band A language on the cut-and-color tier, Band C language on the laser tier. The single document with two strictness levels is the most-asked-for structure across the 2026 sources and reads as fairer than a uniform max.

### Service-Tier-Aware Deposit Schedule

Produce a single table mapping every active `services.cadence_class` to:
- Deposit % (or fixed $)
- When deposit is collected (at booking, 24h before, on arrival)
- Whether deposit is non-refundable, refundable-with-notice, or convertible-to-credit
- Late-cancel treatment (deposit forfeit, partial forfeit, or full credit)
- No-show treatment (forfeit always; the question is whether a future-booking flag is also set)

The skill never writes a uniform percentage across all classes. The schedule must show the differentiation explicitly — that is the change vs. legacy salon policies that quoted one rate across the menu.

### Membership / Pre-Paid Series Carve-Out Rule

Memberships and pre-paid series are economically distinct from per-visit bookings — the client has already paid, and the cancellation question is about *credit treatment*, not deposit forfeit. Always write a separate carve-out paragraph for these:
- If the appointment is part of an active membership benefit, late-cancel inside the window forfeits the visit credit (not a fee).
- No-show forfeits the credit and may pause auto-renewal until the client confirms they want to continue.
- The state-mandated refund-grace window for the membership purchase itself is preserved (3 days in most states; 7 days in CA for some health-related categories).
- Pause / freeze rules (typically allow 1–3 months pause per 12 months) are documented in the membership policy, not the cancellation policy, but cross-referenced.

### Med-Spa Compliance Hook

For any med spa output, also include:
- **Medical-director sign-off line** at the bottom of the policy: "Reviewed by [Medical Director name and credential]." Pull from `config.yml.compliance.medical_director`.
- **Adverse-event escalation note**: if the no-show is a follow-up appointment for a clinical issue (post-laser check, post-injection check), the no-show triggers a clinical follow-up call within 24 hours, not just a billing flag. The policy must state this.
- **Colorado / Arizona / Indiana / Florida / New Jersey 2026 regulatory note** (if `business.location.state` matches): the policy must be prominently displayed in the booking flow and at the treatment room — flag for the medical-director review with the specific state-board citation.
- **HIPAA-aware language**: when describing why a deposit is required (e.g., "to hold the room and the consumables ordered for your treatment"), do not reference the specific service in PHI-exposing detail. Use "for your scheduled treatment" rather than "for your Botox appointment" in any text that may be exported to a third-party booking platform.

Route any med-spa policy output through `operations/ai-consent-and-compliance-guardrails` Review Checklist before publishing. The two skills share a sign-off pattern: this skill produces the policy text; the consent skill produces the consent text; both must be reviewed together to avoid contradiction.

### Platform-Embed Discipline

The same policy must show up consistently in five places:
1. **Website / booking page** — the full canonical text.
2. **Intake-form acknowledgment** — the one-line checkbox: "I have read and accept the cancellation, no-show, and deposit policy linked here."
3. **Booking-confirmation SMS Touch 1** — the one-line policy reference produced by `customer-service/booking-confirmation-sequence` (which reads `business.cancellation_policy` from config). The skill writes the one-liner that fits within Touch 1's segment count.
4. **Front-desk talking-point card** (extended output only) — a wallet-card-sized version with the three sentences a receptionist needs to deliver the policy at check-in.
5. **Reminder SMS Touch 3 soft heads-up** — the within-window reminder language. Produced by `booking-confirmation-sequence`; this skill writes the one-line embed.

The skill writes all five embeds in one run so they are guaranteed consistent. The owner copies them into the respective surfaces.

### Output Structure

Produce, in order:

1. **Headline strictness recommendation** (one sentence): "Recommend Band B / Standard, with Band C strictness on the chair-time-heavy tier." Cite the no-show data driving the recommendation if provided.

2. **Canonical client-facing policy text** (the source-of-truth paragraph for the website / booking page). 200–350 words. Plain English. Active voice. No legalese. Includes the cancellation notice window, the late-cancel treatment, the no-show treatment, the late-arrival grace and re-routing rule (re-route to a shorter service or reschedule, not a discount), the deposit policy by service tier (link to the schedule), the membership / pre-paid carve-out, the new-client first-visit rule, and the refund-grace-window line.

3. **Service-tier deposit schedule table** with the columns described above.

4. **Intake-form acknowledgment line** (one sentence the client checkboxes).

5. **Booking-confirmation SMS Touch 1 one-liner** (single segment, ≤ 160 chars).

6. **Reminder SMS Touch 3 soft heads-up one-liner** (single segment, ≤ 160 chars).

7. **Policy-update announcement** (for existing clients, when the policy is being changed): 30-day grace before the new policy applies, link to the new policy, no fee retroactive. 100–150 words. Send-recommended via email and an SMS one-liner.

8. **Front-desk talking-point card** (extended output only): three sentences for the receptionist. Memorable, calm, non-defensive.

9. **Med-spa compliance footer** (med-spa only): medical-director sign-off line, adverse-event escalation note, state-board flag.

10. **Rationale block** (standard and extended only): one paragraph per choice — why the strictness band, why the deposit schedule shape, why the membership carve-out, what to recheck if the no-show data shifts.

11. **Things to verify before publishing**:
    - Is the booking platform set to actually charge the deposit? (Common gap — policy says yes, platform set to no.)
    - Is the intake-form checkbox wired to the same canonical text? (Common drift point.)
    - Is the website page link in the SMS Touch 1 working and pointing to the same paragraph?
    - Is the medical director's name and credential current on `config.yml.compliance.medical_director`? (Med spa only.)
    - Does any state-specific carve-out apply? (Colorado HB 1024 prominent-display, California 7-day membership refund window, Massachusetts cosmetology scope-of-practice clarifications, Florida SB 1728 separate pharmacy license note for med-spa prescriptions, Indiana 2027 medical-spa registration deadline, New Jersey February 2026 joint-rule injectable oversight.)
    - Has the medical director and the practice's attorney signed off? (Note: this skill does not author the legal review, only the first draft.)

### Voice and Length Rules

- Plain English. No "we reserve the right." No "the Company." No "the Undersigned." No "by signing below the patient consents."
- No threat language. The policy is enforced because the relationship is reciprocal, not because the salon will sue.
- Median sentence under 22 words.
- Standard length under 900 words across all artifacts combined.
- Never name a stylist, provider, or specific client.
- Never quote a state statute verbatim — cite the rule by category (e.g., "Colorado state board prominent-display rule") and route to the medical director / attorney for the citation pull.

### Anti-Pattern Block

Do not:
- Quote one deposit rate across all services. The schedule must be tier-aware.
- Bury the no-show fee in fine print. It must be in the canonical paragraph.
- Reference a specific service by name in the SMS one-liners (PHI exposure for med spa; brand-voice mismatch for salon).
- Use "policy" as a club. The recovery line if a client invokes a hardship is not in this skill's scope — that is `service-recovery-writer`.
- Apply Band C strictness across the menu when the salon-side data only supports Band A or B for short services. Per-tier strictness is the right tool.
- Overpromise refunds. The state-mandated 3-day grace for memberships is a floor, not a ceiling — only widen it with explicit owner approval.
- Make the new-client first-visit deposit higher than the repeat-client deposit on the same service. New clients haven't broken trust yet, and the deposit there is risk-management, not punishment.

### Routing Map — When Another Skill Owns the Job

| Question / situation | Owning skill |
|---|---|
| Live client complaint about being charged a no-show fee | `customer-service/service-recovery-writer` (recovery message) |
| Repeat-no-show client re-engagement after the deposit-required flag | `customer-service/client-winback-sequence` (no-show-repeat tier) |
| Same-day fill of the slot the no-show vacated | `operations/waitlist-gap-fill-outreach` |
| Reminder SMS cadence that prevents the no-show in the first place | `customer-service/booking-confirmation-sequence` (Touch 0 / 1 / 2 / 3) |
| Owner decision to tighten policy because the no-show rate spiked | `operations/weekly-kpi-owner-briefing` flagged the trend; this skill writes the rewrite |
| AI / privacy disclosure (separate from cancellation) | `operations/ai-consent-and-compliance-guardrails` |
| New-client first-visit policy framing | `customer-service/new-client-welcome-journey` (welcome side); this skill writes the deposit-rule side |
| Membership / subscription cancellation rules | (future) `sales/membership-program-builder`. For now, this skill carries the cancellation carve-out and flags the membership-rules question for the program-builder skill once it lands |
| Public review citing the policy | `customer-service/review-response-writer` (the recovery-style platform reply) |

## Worked Example — Salon X, Boston MA, hybrid hair + lash + simple med-spa add-on tier

Service mix: cuts ($65, 45 min) / color & balayage ($180–$280, 2–3 hr) / lash extensions full set ($220, 2 hr) / lash fills ($95, 60 min) / a small dermaplaning add-on tier ($150, 45 min, NP-supervised). Last-90-day data: no-shows 6% on cuts, 11% on color, 14% on lash extensions, 8% on lash fills, 19% on dermaplaning. Booking platform supports deposit-on-booking and card-on-file. State: Massachusetts (post-May 2025 cosmetology / med-spa scope clarification).

**Recommendation**: Per-tier strictness. Band A on cuts (the relationship is the enforcement). Band B on color and lash fills. Band C on lash extensions and dermaplaning, with deposit-required-at-booking on those two tiers. Medical-director sign-off line for the dermaplaning row only. Cite the May 2025 MA cosmetology / med-spa scope clarification in the rationale block.

**Deposit schedule**:

| Service tier | Deposit | When collected | Refund treatment | Late-cancel | No-show |
|---|---|---|---|---|---|
| Cuts | none (card on file) | n/a | n/a | 50% of service | 100% of service |
| Color / balayage | 25% (~$45–$70) | at booking | refundable with 24h notice | deposit forfeit | full forfeit + future-booking deposit flag after 2nd in 12 mo |
| Lash fills | 20% (~$19) | at booking | refundable with 24h notice | deposit forfeit | full forfeit + future-booking deposit flag after 2nd in 12 mo |
| Lash extensions full set | 50% (~$110) | at booking | refundable with 48h notice | deposit forfeit | full forfeit + future-booking deposit flag after 1st |
| Dermaplaning (NP-supervised) | 50% (~$75) | at booking | refundable with 48h notice | deposit forfeit | full forfeit + future-booking deposit flag after 1st + 24h clinical follow-up call |

**Intake checkbox**: "I have read and accept Salon X's cancellation, no-show, and deposit policy. I understand that booking a color, lash, or dermaplaning service requires a deposit at booking and that the deposit terms vary by service."

**SMS Touch 1 one-liner**: "Confirming your [service] tomorrow at [time] — need to move it? Reply here. Cancellation window: 24 hr / 48 hr for lash and dermaplaning. Reply STOP to opt out."

**Policy-update announcement**: 30-day grace before the deposit-on-booking rule applies to existing color and lash extension clients; existing dermaplaning clients are notified that the deposit-on-booking rule applies starting at their next visit; no retroactive fee for any past cancellation.

**Things to verify**: Is the platform actually configured to charge the deposit at booking on the color row? (Common gap.) Is the dermaplaning row's medical-director sign-off line current on `config.yml.compliance.medical_director`? Is the May 2025 MA cosmetology / med-spa scope clarification cited in the front-desk training? Has the practice's attorney signed off on the dermaplaning row's 24-hour clinical follow-up call language?

## Inputs / Outputs Summary

- **Inputs:** business profile, service tier breakdown, current policy (if any), no-show / late-cancel benchmarks, deposit-collection capability, tone preference, output preference.
- **Outputs:** headline recommendation, canonical client-facing policy text, service-tier deposit schedule, intake-form acknowledgment line, SMS Touch 1 one-liner, SMS Touch 3 soft heads-up one-liner, policy-update announcement, front-desk talking-point card (extended), med-spa compliance footer (med spa only), rationale block, things-to-verify checklist.

## Review Before Publishing
- Practice's CPA / employment attorney for the deposit-forfeit and refund-grace language.
- Medical director (med spa only) for the dermaplaning / laser / neuromod / filler tiers and the adverse-event escalation note.
- Booking-platform admin to verify the deposit-on-booking rule is actually wired.
- Front-desk lead to verify the talking-point card sounds like the team and not like the policy paper.

This skill produces a first draft. Final publish requires the practice's professional advisors.
