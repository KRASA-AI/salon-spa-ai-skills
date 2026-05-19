---
name: "No-Show Risk Reminder"
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/day"
version: 1.1
last_eval_score: 8.4
---

# No-Show Risk Reminder

## Purpose

Draft a personalized confirmation + reminder sequence for an upcoming appointment based on **how likely that specific client is to no-show**. Higher-risk bookings get more touchpoints, an earlier confirmation, and a clearer cancellation-fee callout. Lower-risk bookings get a single warm reminder and stay out of the client's inbox.

The goal is fewer no-shows without spamming reliable regulars.

This is the **per-client risk-graded** sequence. For the standard pre-arrival confirmation cadence used on every booking regardless of risk, use `customer-service/booking-confirmation-sequence` instead — see the routing block at the bottom of this file.

**Cross-skill state inheritance**: When this skill is invoked after `operations/cancellation-no-show-policy-author` has produced the canonical policy pack, the policy paragraph and the service-tier-aware deposit / fee dollar amount are piped directly into the High-tier T-72h confirmation and T+15min follow-up — verbatim. No generic "per our policy" placeholder.

## When to Use

- Daily, at the start of the shift, on the next 24–72 hours of bookings
- Whenever a new client (no history) books a high-value service
- When the front desk is rebuilding a sequence after a software change

## Required Input

For each appointment in scope, the user provides whatever subset of these signals is available — the skill works with partial data:

- Client name and contact channel preference (SMS / email / both)
- Service, duration, price, and deposit status
- Booking lead time (booked X days ago)
- Prior no-show count and prior late-cancel count (last 12 months)
- Whether this is a first visit
- Anything notable in their booking history (recurring weekly client, books and reschedules a lot, large package buyer, etc.)
- **Upstream input** (optional): the output of `operations/cancellation-no-show-policy-author` — the skill will pull the exact client-facing policy paragraph, the cadence-class deposit table, and the fee dollar amount from the policy pack rather than referencing "per our policy"

If only a list of names + services is given, the skill will assume "unknown history" and produce the default cadence.

## Instructions

You are an operations assistant for a salon, spa, or med spa. Score each booking's no-show risk and produce a reminder plan that matches the risk level.

**Before you start:**
- Load `config.yml` for company name, voice, cancellation policy, deposit rules, and service cadence classes
- Use the contact channel and timing rules from `config.yml` → `voice` → `followup_style` as the default cadence; override per-client as described below

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Sign-off and brand voice in every message | "the salon" |
| `business.business_type` | Determines whether the Med-Spa T-72h Pre-Check expansion applies and whether prep notes include clinical instructions | infer from cadence class |
| `voice.tone` | Message register (warm-conversational, warm-professional, clinical-professional, upbeat-modern, concise-transactional) | warm-professional |
| `voice.followup_style.contact_channel` | Default contact channel (SMS / email / both) | SMS for High-tier, email for Low-tier, follow user preference for Medium |
| `pricing.cancellation_policy.client_facing_text` | Verbatim policy paragraph used in High-tier T-72h confirmation and T+15min follow-up | generic "Same-day cancels are charged per our cancellation policy" placeholder — flag for user |
| `pricing.cancellation_policy.deposit_schedule` | Per-cadence-class deposit and fee dollar amount surfaced in the message body | omit dollar amount; flag for user |
| `services.cadence_class` | Day-Of Prep-Note Table — selects the correct prep note for the booked service | generic "see you soon" with no prep |
| `services.menu` | Refer to the booked service by the practice's exact menu name | use the term the user gave you |
| `staff.roster` | Reference the assigned provider by first name and role | "your provider" |
| `compliance.regulated_language` | Required disclosures for med-spa-* services in the T-72h pre-check | omit the disclosure; flag for user — never invent regulated language |

If a required config key is missing, the skill still produces — flag the gap in the per-client "Why this tier" line ("Generic policy text used — policy author has not been run") rather than inventing dollar amounts or regulated language.

### Risk Scoring — Assign Each Appointment a Tier

| Tier | Signals (any one is enough) |
|------|------------------------------|
| **High** | First-time client AND no deposit. Or: 1+ no-show in last 12 months. Or: 2+ late cancels in last 12 months. Or: booked more than 21 days out with no deposit. Or: any `med-spa-*` cadence class with no deposit (clinical-tier never goes to standard cadence). |
| **Medium** | New client with deposit on file. Or: 1 late cancel in last 12 months. Or: booked 7–21 days out. Or: history unknown. |
| **Low** | Repeat client with zero no-shows / late cancels in last 12 months. Or: weekly / biweekly standing appointment. Or: pre-paid package. |

### Cadence Per Tier

- **High** — 4 touchpoints
  1. T-72h: confirmation request (must reply Y/N) with cancellation-window callout — pull the verbatim policy paragraph and deposit / fee dollar amount from `config.yml.pricing.cancellation_policy`
  2. T-24h: reminder + day-of prep note from the cadence-class table below
  3. T-2h: final reminder
  4. T+15min if no-show: warm but firm follow-up offering one reschedule, then trigger fee per policy — pull the exact fee dollar amount from the policy pack

- **Medium** — 2 touchpoints
  1. T-48h: confirmation (Y/N reply optional)
  2. T-2h: reminder

- **Low** — 1 touchpoint
  1. T-24h: short, warm reminder. No cancellation policy language unless they reply asking to reschedule.

### Day-Of Prep-Note Table

Day-of prep notes are pulled from this table keyed to `services.cadence_class`. Use only the prep notes relevant to the booked service. Never copy a prep instruction from a different cadence class.

| Cadence class | Day-of prep note |
|---|---|
| `color-cadence` | "Come with dry, unwashed hair unless instructed otherwise. No serums or styling products on the lengths today." |
| `lash-lift` | "Please come without eye makeup or mascara. Skip caffeine in the 2 hours before your appointment if you're sensitive." |
| `brow-tint` | "No makeup or moisturizer on your brows today. Let us know if you've used a retinoid on your forehead in the last 7 days." |
| `gel-manicure` | "Come with bare nails if possible — if you have product on, we'll remove it." |
| `nail-extension` | "Come with bare nails. Bring any reference photos for shape and length." |
| `facial-cadence` | "Come with clean skin — no SPF or makeup. Skip retinoids tonight. Hydrate today." |
| `body-cadence` | "Hydrate today. Avoid a heavy meal in the hour before. Let us know if anything has changed health-wise since booking." |
| `med-spa-laser` | "Come with clean skin in the treatment area — no SPF, no makeup, no lotion. Avoid any active tan. If you've started a photosensitizing medication since booking, please call us before arriving." |
| `med-spa-neuromod` | "Avoid alcohol the night before and the morning of. Skip NSAIDs (ibuprofen / aspirin) for 24h before if cleared by your prescriber. Plan no major physical exertion for 4 hours after." |
| `med-spa-filler` | "Avoid alcohol, NSAIDs, fish oil, and vitamin E for 48h before. Plan low-impact activity for the next 24h. If you've had dental work in the last 2 weeks or anticipate any in the next 2, please call us before arriving." |
| `haircut-style` | "Bring any reference photos. Come with your hair dry and styled the way you usually wear it." |
| `bridal` | "Wear a button-down or zip-front top so you can change without disturbing the styling. Bring your hairpiece, veil, or accessories." |

If `cadence_class` is missing or unrecognized, default to a generic "See you soon — let us know if anything has changed since you booked" rather than guessing prep instructions.

### Med-Spa T-72h Pre-Check Expansion

For any service with a `med-spa-*` cadence class, the High-tier T-72h confirmation message is expanded with a clinical pre-check question. This is not optional. The pre-check asks the client to confirm there have been no changes to medications, photosensitizing prescriptions (laser), anticoagulants (filler), pregnancy status, or active skin conditions in the treatment area since their booking. If the client volunteers any change, the front desk must escalate to the assigned provider before confirming the appointment.

The T-72h clinical pre-check uses `config.yml.compliance.regulated_language` for required disclosures. Never invent regulated language — if the config key is missing, flag it for the user and use a non-regulated paraphrase.

### Drafting Rules

- Use the client's first name. Never "Hi there" or "Dear customer".
- Reference the actual service and time. Not "your appointment".
- Match the voice from `config.yml.voice.tone` — if the brand is warm and casual, write warm and casual. If it's clinical / medical, write clean and precise.
- Include the cancellation policy paragraph **only** on High-tier T-72h confirmation and the T+15min follow-up. Other tiers don't need it.
- Day-of prep notes must come from the cadence-class table above. Never mix instructions across classes.
- Keep each SMS message under 320 characters where SMS is the channel.
- Never invent a deposit amount or policy detail not in `config.yml`. If the policy author has not been run, write `[deposit / fee amount — pending policy author]` in brackets and flag in the summary.

**Output format:**

```
NO-SHOW PLAN — [date range covered]

Summary
- High risk: [count]   Medium: [count]   Low: [count]
- Estimated revenue at risk (high tier only): [sum of service prices]
- Policy source: [config.yml.pricing.cancellation_policy / cancellation-no-show-policy-author output / placeholder — flag if placeholder]

—

[Client name] — [service] — [appt time] — Tier: [HIGH / MEDIUM / LOW]
Why this tier: [one-line reason, plus any flagged config gap]

Message 1 (send [when], channel [SMS / email]):
"[message]"

Message 2 (send [when], channel [SMS / email]):
"[message]"

[...continue for the tier's cadence]

—

[next client...]

Top intervention for today: [the single highest-leverage thing the front desk should do — e.g., "Call Marisa directly, $480 booking, no deposit, two prior no-shows"]
```

## Example Output

```
NO-SHOW PLAN — Thu 6/4 through Sat 6/6

Summary
- High risk: 2   Medium: 5   Low: 8
- Estimated revenue at risk (high tier only): $710
- Policy source: config.yml.pricing.cancellation_policy (from cancellation-no-show-policy-author v1.0)

—

Marisa K. — Hydrafacial + LED add-on — Sat 6/6 11:00am — Tier: HIGH
Why this tier: 2 prior no-shows in last 12 mo, $230 service, no deposit on file (med-spa cadence class — clinical-tier requires deposit; flag for front desk).

Message 1 (send Thu 11:00am, channel SMS):
"Hi Marisa — confirming your Hydrafacial + LED with Devon on Saturday at 11am ($230). Reply Y to confirm or text RESCHED to move it. Per our policy, same-day cancels are charged 50% ($115) and a no-show is the full $230. Quick clinical check — any new medications, retinoids, or active sun exposure since booking? Reply N if no changes."

Message 2 (send Fri 11:00am, channel SMS):
"Hey Marisa — quick reminder you're booked with Devon tomorrow at 11am. Come with clean skin — no SPF or makeup — and skip retinoids tonight. See you soon."

Message 3 (send Sat 9:00am, channel SMS):
"Morning Marisa — you're up at 11. Parking is in the lot behind the building. Text us if anything changes."

Message 4 (send Sat 11:15am IF no-show, channel SMS):
"Hi Marisa — we missed you at 11. Want to reschedule for next week? We can hold one make-good slot. Per our policy a $230 no-show fee applies for today; let us know how you'd like to proceed and we'll work with you on the rescheduled visit."

—

Top intervention for today: Call Marisa directly before the 11:00am SMS goes out — two prior no-shows on a $230 med-spa booking with no deposit means a 30-second human call beats any automation. Also: flag the front desk to add a deposit policy on med-spa-* cadence classes per cancellation-no-show-policy-author guidance.
```

## When Another Skill Owns This Job

| Scenario | Owner skill |
|---|---|
| Standard pre-arrival confirmation cadence for a regular booking (no per-client risk grading needed) | `customer-service/booking-confirmation-sequence` |
| The cancellation policy text and deposit schedule themselves need to be written or updated | `operations/cancellation-no-show-policy-author` |
| A High-tier client did no-show and the fee conversation needs a recovery message | `customer-service/service-recovery-writer` |
| A High-tier client cancels last minute and you need to fill the slot | `operations/waitlist-gap-fill-outreach` |
| A repeat no-show client is being re-engaged after a lapse | `customer-service/client-winback-sequence` |
| The post-visit after-care or product follow-up needs drafting | `_shared/email-drafter` (archetype: post-visit after-care) |
| The morning's at-risk bookings need to be surfaced to the owner as a daily lever | `operations/morning-owner-brief` (this skill's per-client output feeds the "Risk spots" row) |

## Notes

- This skill drafts language for a human to send. It does not connect to your booking system. If you want auto-send, wire the drafts into your existing SMS or email platform.
- Risk tiers are conservative defaults. Adjust thresholds in this skill once you have 60+ days of your own data on which appointments actually no-show.
- Never auto-send a cancellation fee. Always have a human review the T+15min message before it goes out.
- For med-spa cadence classes, never produce a sequence that omits the T-72h clinical pre-check. A missing answer is escalated to the provider, not silently passed.
