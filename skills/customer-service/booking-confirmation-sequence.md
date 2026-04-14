---
name: Booking Confirmation Sequence
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min/booking"
version: 2.0
last_eval_score: 4.9
---

# Booking Confirmation Sequence

## Purpose
Produce the full multi-touch messaging sequence that turns a new booking into a confirmed, well-prepared, on-time client: the instant confirmation, the prep instructions, the reminder, and the day-of welcome. Calibrated for SMS and email channels with salon/spa-specific language, correct pre-service instructions by service type, and compliance-safe opt-out handling.

## When to Use
- A new appointment is booked (online, by phone, or at checkout for next visit).
- A service type requires specific pre-visit prep (e.g., come with clean dry hair, no caffeine before a facial, arrive with bare nails).
- Reducing no-show / late-arrival rates on a specific service or day-part.
- Onboarding a first-time client whose expectations need setting.
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

Load business context from `config.yml` (business name, address, parking notes, cancellation policy, voice) and reference `knowledge-base/terminology/` for correct service and product names.

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
- Service-specific prep rules (use the matching block):

  - **Hair color / highlights / balayage**: Come with hair dry and product-free the day of (or washed 24 hours prior for color services, fully dry). No buns/tight ponytails for at least 4 hours before — they flatten the natural fall. Bring inspiration photos if you have them.
  - **Keratin / smoothing / chemical services**: Hair should be clean, completely dry, and free of product. You will not be able to wash, tie up, or tuck behind ears for 48–72 hours after — plan wardrobe and accessories accordingly.
  - **Haircut / blow-dry / style**: Come with hair freshly washed if possible. Let us know if you'd like us to shampoo as part of the service.
  - **Facial**: Skip retinoids, exfoliants, and waxing for 5 days prior. Arrive with a clean face if possible. Avoid caffeine in the 2 hours before if you have sensitive skin. Hydrate well.
  - **Massage / body treatment**: Eat a light meal about 90 minutes prior. Skip heavy perfumes. Arrive 10 minutes early to settle in.
  - **Lash extensions / lift**: Come with clean, mascara-free lashes. Avoid caffeine before to reduce eye flutter. Avoid lash serums for 24 hours prior.
  - **Brows (wax, tint, lamination)**: Skip retinoids and exfoliants for 3 days. Do not tweeze for 2–3 weeks before a first shaping visit.
  - **Nails (manicure, gel, extensions)**: Come with bare nails if possible — removal is a separate service. Bring inspiration for color or art.
  - **Bridal / event**: Provide reference photos 48 hours ahead if not already shared. For trial: come with hair/makeup as you typically wear it so we can see your starting point.

- Must also include: arrival guidance (where to park, which entrance), what to bring (ID, reference photos, credit card for services requiring card on file), any intake forms to complete, who to contact if running late.
- Tone: helpful, confidence-building. Lead with the 1–2 most important prep items for their service, then the rest.

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

### Global Rules
- Always use the client's first name (never "customer" or "valued client").
- Never send the same message on two channels at the same time — if sending both SMS and email, the SMS should be shorter and link to the email for fuller detail.
- Cancellation policy is mentioned once in Touch 1 and once (softly) in Touch 3 — not every touch.
- Include opt-out language on every SMS in compliance with TCPA / local regulations ("Reply STOP to opt out").
- If a deposit was collected, mention it and the refund policy in Touch 1 only.

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
