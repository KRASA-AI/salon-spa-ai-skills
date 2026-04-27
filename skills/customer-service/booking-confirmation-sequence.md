---
name: Booking Confirmation Sequence
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/booking"
version: 2.1
last_eval_score: 9.1
---

# Booking Confirmation Sequence

## Purpose
Produce the full multi-touch messaging sequence that turns a new booking into a confirmed, well-prepared, on-time client: the instant confirmation, the prep instructions, the reminder, and the day-of welcome. Calibrated for SMS and email channels with salon/spa-specific language, correct pre-service instructions by service type, and compliance-safe opt-out handling.

This skill is the **transactional** track for a single appointment. It runs in parallel with the **relational** track in `customer-service/new-client-welcome-journey` for first-time clients — the welcome journey adds a separate warm note in the 24–48 hour window that this sequence does not fill, and the two tracks should not share a thread. For lapsed-client re-engagement, use `customer-service/client-winback-sequence`. For clinical retreatment cadence (Botox, filler, peel series, etc.), the Touch-4 cadence-class handoff in this skill triggers `customer-service/treatment-cadence-rebooking`.

## When to Use
- A new appointment is booked (online, by phone, or at checkout for next visit).
- A service type requires specific pre-visit prep (e.g., come with clean dry hair, no caffeine before a facial, arrive with bare nails).
- Reducing no-show / late-arrival rates on a specific service or day-part.
- Onboarding a first-time client whose expectations need setting (relational track is parallel, not in this thread).
- Rebooking a client after a no-show (see client-winback-sequence for the lapsed variant).

## Required Input
- **Client first name**
- **Service(s) booked** — the exact menu item (e.g., "Balayage + toner + blow-dry")
- **Appointment date and time** (with timezone if relevant)
- **Provider name**
- **Location** (for multi-location businesses)
- **Estimated duration and total** (if the booking system produces these)
- **New client?** Yes/No — affects welcome tone and parking/entry details
- **Channel(s)** — SMS, email, or both
- **Deposit or pre-payment status** (if applicable)

## Instructions

You are a front-of-house communications writer for a salon or spa. Your job is to draft a 4-message booking sequence that is warm, specific, and never spammy. Every touch earns its place by delivering information the client actually needs.

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service and product names. For med-spa bookings (any service category mapped to a clinical/med-spa cadence class in the prep map below), pass the final draft through the `operations/ai-consent-and-compliance-guardrails` Review Checklist before send (specifically: no promotional reference to a prescription product, no diagnosis/medication/adverse-event language, channel-specific marketing authorization on file, working opt-out present).

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Subject line branding, sign-off | "the salon" |
| `business.address` | Touch 1 location line, Touch 2 directions block | omit if unknown |
| `business.parking_notes` | Touch 2 arrival guidance ("street parking opens up after 1 PM") | omit |
| `business.cancellation_policy` | Touch 1 one-line policy, Touch 3 soft heads-up | "24-hour cancellation window" |
| `business.voice.tone` | Base register for every touch | warm-professional |
| `business.voice.always_use` / `never_use` | Voice constraints honored across all four touches | — |
| `business.opt_out_footer` | Required SMS / email footer | "Reply STOP to opt out" |
| `services.menu` | Exact service name on Touch 1 (no shorthand) | ask user |
| `services.cadence_class` | Maps booked service to a cadence class (see Service Category Prep Map) — drives Touch 4 cadence-class handoff and triggers the `treatment-cadence-rebooking` schedule for cadence-tracked services | "untracked" (no cadence handoff) |
| `staff.roster[]` | Provider name + role for Touch 1 attribution | "your provider" |
| `pricing.payment_terms` | Whether deposit was collected; refund-policy line on Touch 1 | omit |
| `compliance.regulated_language` | Med-spa: required language on Touch 1 / 2 for clinical bookings | omit (non-clinical business) |

If a required config key is missing, flag it in the deliverable's "Notes for sender" rather than inventing.

### The 4-Touch Sequence

**Touch 1 — Instant Confirmation (sent within 5 minutes of booking)**
- Channel: email (richer details) and/or SMS (short acknowledgment)
- Purpose: reassure the client that the booking went through and set first expectations
- Must include: client name, exact service, date/time, provider, location, duration estimate, price estimate (if disclosed), cancellation policy one-liner, reschedule link
- Tone: warm, brief. No pre-visit instructions yet — that is Touch 2's job.
- SMS length: 140–160 chars. Email: 120–180 words.

**Touch 2 — Prep Instructions (sent 48 hours before appointment)**
- Channel: email preferred (more room for service-specific detail). SMS-only businesses: a short SMS with the 2–3 most important prep items.
- Purpose: deliver service-specific prep so the appointment runs smoothly
- Use the **Service Category Prep Map** below to pick the matching block. Each row carries a prep block, an aftercare hint to share with new clients, and a cadence class that drives the Touch 4 handoff to `treatment-cadence-rebooking`.

#### Service Category Prep Map (16 categories)

| Service category | Prep block | Aftercare hint (new clients) | Cadence class |
|---|---|---|---|
| **Hair color / highlights / balayage** | Come with hair dry and product-free the day of (or washed 24 hours prior, fully dry). No buns or tight ponytails for at least 4 hours before — they flatten the natural fall. Bring inspiration photos if you have them. | Wait 48 hours before first shampoo; use color-safe sulfate-free shampoo. | hair-color (8–12 wk) |
| **Keratin / smoothing / chemical services** | Hair should be clean, completely dry, and free of product. You will not be able to wash, tie up, or tuck behind ears for 48–72 hours after — plan wardrobe and accessories accordingly. | No water, ponytails, or pillow lines for 72 hours. | hair-chemical (12–16 wk) |
| **Haircut / blow-dry / style** | Come with hair freshly washed if possible. Let us know if you'd like us to shampoo as part of the service. | Heat-protectant before any styling tool; trim every 6–8 weeks for shape. | haircut (4–8 wk) |
| **Facial** | Skip retinoids, exfoliants, and waxing for 5 days prior. Arrive with a clean face if possible. Avoid caffeine in the 2 hours before if you have sensitive skin. Hydrate well. | No active acids or retinoids for 48 hours; SPF every morning. | facial (4 wk) |
| **Clinical peel / microneedling / RF microneedling** | No retinoids, exfoliants, or AHAs/BHAs for 7 days prior. No sun exposure 7 days prior; no waxing or threading in the treatment area for 14 days prior. Disclose any active cold sores or open lesions before booking. | Strict SPF 50+ daily for 14 days; no actives, no sun, no makeup for 24–48 hours per provider's specific direction. | clinical-peel (4–6 wk; series of 3) |
| **Massage / body treatment** | Eat a light meal about 90 minutes prior. Skip heavy perfumes. Arrive 10 minutes early to settle in. Hydrate generously after. | Hydrate; avoid heavy gym sessions same day. | bodywork (4–6 wk) |
| **Lash extensions / fills** | Come with clean, mascara-free lashes. Avoid caffeine before to reduce eye flutter. Avoid lash serums for 24 hours prior. | Keep lashes dry for 24 hours; no oil-based products near the eyes; brush with a clean spoolie daily. | lash-extensions (2–3 wk fill) |
| **Lash lift / tint** | Clean lashes, no mascara, no waterline liner. Skip lash serums 24 hours prior. | No water on lashes for 24 hours; no oil-based eye products. | lash-lift (6–8 wk) |
| **Brows (wax, tint, lamination)** | Skip retinoids and exfoliants for 3 days. Do not tweeze for 2–3 weeks before a first shaping visit. | No water/sweat for 24 hours after lamination; brush daily. | brows (3–5 wk) |
| **Nails (manicure, gel)** | Come with bare nails if possible — removal is a separate service. Bring inspiration for color or art. | Cuticle oil daily; gloves for cleaning chemicals. | nails (3–4 wk) |
| **Nail extensions (acrylic / hard gel / dip)** | Bare nails or existing extensions in good repair; removal is a separate service and adds 30+ min to the appointment. Bring shape and color inspiration. | Avoid prying objects with the nails; cuticle oil daily; book fills before lift starts. | nail-extensions (2–3 wk fill) |
| **Bridal / event** | Provide reference photos 48 hours ahead if not already shared. For the trial: come with hair / makeup as you typically wear it so we can see your starting point. | Day-of prep brief sent 7 days before the event; trial recommended 4–8 weeks ahead. | bridal-trial (one-off; trial 4–8 wk pre-event) |
| **Med-spa neuromodulators (Botox / Dysport / Daxxify / Xeomin)** | Avoid blood-thinning supplements (vitamin E, fish oil, ginkgo, ibuprofen) for 3–5 days prior unless contraindicated by your physician. No alcohol 24 hours prior. Disclose pregnancy or breastfeeding status. Skip strenuous exercise the day-of. | No lying flat or vigorous exercise for 4 hours; no facials or massage to the area for 7 days. Results visible in 5–14 days. | med-spa-neuromodulator (10–14 wk; Daxxify 16–24 wk) |
| **Med-spa fillers (HA — Juvederm / Restylane / RHA)** | Avoid blood-thinners (per neuromodulator list) for 3–5 days; no alcohol 24 hours prior; arrive without lipstick or heavy makeup; bring a list of any current medications. Disclose any planned dental work or vaccinations within 2 weeks of the appointment. | Ice the area in 10-min intervals on the day of treatment; no facials or massage to the area for 14 days; expect swelling and possible bruising for 5–10 days. | med-spa-filler (6–12 mo, site-dependent) |
| **Med-spa laser / IPL / RF / laser hair removal** | No sun exposure or self-tanner in the treatment area for 14 days prior. No waxing, threading, or epilating in the area for 4 weeks (shaving is fine). Disclose any photosensitive medications (doxycycline, isotretinoin, etc.). | Aloe and SPF 50+ for 7–14 days post; no exfoliants or actives for 7 days; no hot showers or saunas for 24 hours. | med-spa-laser (4–10 wk depending on body site / device) |
| **Multi-service combo (color + cut + treatment, facial + brow, etc.)** | Use the prep block for the most prep-sensitive sub-service. Allow extra arrival buffer (5–10 min). Confirm whether services run in parallel or back-to-back so you can plan the day around the longer block. | Aftercare per the most aftercare-sensitive sub-service. | combo (defer to longest-cadence sub-service) |

- Must also include: arrival guidance (where to park, which entrance — pull from `business.parking_notes`), what to bring (ID, reference photos, credit card if a card on file is required), any intake forms to complete, who to contact if running late.
- Tone: helpful, confidence-building. Lead with the 1–2 most important prep items for their service, then the rest.

#### Same-Day Booking Collapse Rule

If the appointment is booked on the same day (less than 24 hours from now), Touch 2 (48-hour prep) and Touch 3 (24-hour reminder) collapse into a single send delivered immediately after Touch 1: the most critical 1–2 prep items + the day-of arrival details. Do not send a separate "day-before reminder" the night before — by then the client is mid-prep and a second touch reads as redundant. The Touch 4 day-of welcome still fires normally.

**Touch 3 — Day-Before Reminder (sent ~24 hours before)**
- Channel: SMS (short, reply-friendly)
- Purpose: confirm they will be there; open a reschedule path before cancellation-fee windows
- Must include: service, date/time, provider name, reschedule link or reply instruction, cancellation-window reminder phrased as a soft heads-up (not a threat)
- Tone: casual, friendly. Example: "Hi Jamie! Just confirming tomorrow at 2pm with Maya for your balayage. See you then! Need to shift the time? Reply here or book with the link: [link]"
- SMS length: under 155 chars, plus opt-out line if legally required.

**Touch 4 — Day-Of Welcome (sent 2–3 hours before appointment)**
- Channel: SMS
- Purpose: final nudge with entry details and small warm touch
- Must include: exact arrival time (including any "arrive 10 minutes early" ask), entry/parking reminder, offer of help if running late
- Tone: warm, brief. No new info that the client needed earlier.
- SMS length: under 140 chars.

**Touch 4 (post-visit) — Cadence-Class Handoff Trigger (internal scheduling, fires at end of visit)**
- Not a client-facing send. At appointment close, look up the booked service in the Service Category Prep Map's cadence-class column.
- For any cadence-tracked class (everything except `untracked`, `combo`-deferred, and `bridal-trial`), schedule a `customer-service/treatment-cadence-rebooking` trigger to fire at `next_visit_window_open − 7 days`. The cadence-class column carries the optimal retreatment window; the `treatment-cadence-rebooking` skill then runs its own three-touch sequence keyed to clinical "why" copy.
- For `untracked` (one-off bookings, untracked categories) and `combo` services where the cadence class defers to the longest sub-service, fall back to the salon's standard reminder cadence rather than triggering `treatment-cadence-rebooking`.
- For new clients, the cadence-class trigger is informational only on the first visit — the relational `customer-service/new-client-welcome-journey` track owns the second-visit nudge for the first 90 days. After the second visit, cadence-class triggers resume normally.

### Parallel-Track Note (New Clients)

For first-time clients, this skill stays transactional — the four touches deliver booking confirmation, prep, reminder, and arrival. The `customer-service/new-client-welcome-journey` skill runs in parallel as a relational track, adding a separate provider-attributed welcome note in the 24–48 hour window between this sequence's Touch 1 and Touch 2. Do NOT mix the two threads — relational and transactional content read worse blended than as two distinct moments. Each track's pause/escalate triggers run independently.

### Med-Spa Compliance Review Hook

Any service mapped to a `med-spa-*` cadence class (neuromodulator / filler / laser) routes the final Touch 1 and Touch 2 drafts through the `operations/ai-consent-and-compliance-guardrails` Human-in-the-Loop Review Checklist before send. Specifically: confirm the draft contains no promotional reference to a prescription product (item 2), no diagnosis or adverse-event language (item 3), the client's marketing-channel authorization is on file (item 4), and the EU AI Act Article 50 notice (Artifact 4) appends to Touch 1 for any client whose chart flag indicates EU residency. The Review Checklist sign-off is logged with the booking record.

### Global Rules
- Always use the client's first name (never "customer" or "valued client").
- Never send the same message on two channels at the same time — if sending both SMS and email, the SMS should be shorter and link to the email for fuller detail.
- Cancellation policy is mentioned once in Touch 1 and once (softly) in Touch 3 — not every touch.
- Include opt-out language on every SMS in compliance with TCPA / local regulations ("Reply STOP to opt out").
- If a deposit was collected, mention it and the refund policy in Touch 1 only.
- Same-day bookings: Touch 2 + Touch 3 collapse into one (see Same-Day Booking Collapse Rule above); Touch 4 (day-of) and the post-visit cadence-class trigger still fire normally.

### Output Format
Return all four touches clearly labeled by touch number and channel. Each touch block should be:
- **Channel:** (SMS / Email)
- **Send timing:** relative to booking/appointment
- **Subject line:** (for email)
- **Message body:** ready to send, with variables shown in `{{curly braces}}` where the booking system will substitute values

## Example Output — Balayage Booking for a New Client

**Touch 1 — Instant Confirmation (Email)**
- **Send timing:** within 5 minutes of booking
- **Subject line:** See you April 27 — your balayage is booked ✨
- **Body:**

Hi Jamie,

Your appointment is confirmed!

**Service:** Balayage + toner + blow-dry with Maya
**Date & time:** Monday, April 27 · 2:00 PM
**Duration:** ~2 hours 30 minutes
**Estimate:** $235–$275 (finalized in consultation)
**Location:** [Salon Name], [Address]

We'll send prep instructions two days before your visit so your color comes out exactly the way you want. If you need to shift anything, you can reschedule here: [link]. Our cancellation window is 24 hours.

Can't wait to have you in,
The [Salon Name] team

---

**Touch 1 — Instant Confirmation (SMS, optional if client is SMS-only)**
- **Send timing:** within 5 minutes of booking
- **Body:** `Hi Jamie! Your balayage with Maya is booked for Mon Apr 27 at 2pm. Full details + prep coming by email. Manage booking: [link]  Reply STOP to opt out.`

---

**Touch 2 — Prep Instructions (Email)**
- **Send timing:** 48 hours before appointment
- **Subject line:** A few things before your balayage on Monday
- **Body:**

Hi Jamie,

Your balayage is Monday at 2:00 PM with Maya. A quick prep list so we can give you the best result:

**The most important things**
1. Come with your hair dry and product-free — clean works, but no pomades, dry shampoo, or leave-ins.
2. Skip tight ponytails or buns the morning of. They flatten how your hair naturally falls and that's how Maya plans placement.
3. Bring inspiration photos if you have them — saved on your phone is perfect.

**Arriving**
We're at [Address]. Street parking is usually easy after 1 PM on Monday, and there's a paid lot at [nearby garage]. Please arrive 5 minutes early for check-in and a quick consult with Maya.

**If you're running late**
Text us at [salon phone] and we'll hold the time if we can.

See you Monday!
The [Salon Name] team

---

**Touch 3 — Day-Before Reminder (SMS)**
- **Send timing:** 24 hours before appointment
- **Body:** `Hi Jamie! Just confirming your balayage with Maya tomorrow (Mon) at 2pm. Need to move it? Reply here or [link]. Our cancellation window is 24 hrs. Reply STOP to opt out.`

---

**Touch 4 — Day-Of Welcome (SMS)**
- **Send timing:** 2–3 hours before appointment
- **Body:** `Hi Jamie — see you at 2pm! Street parking usually opens up after 1pm, and we're the navy door on [street]. Running late? Just text us. 💛`
