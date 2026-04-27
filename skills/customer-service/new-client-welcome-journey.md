---
name: New Client Welcome & First-90-Day Retention Journey
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/new-client cohort"
version: 1.0
last_eval_score: 9.3
---

# New Client Welcome & First-90-Day Retention Journey

## Purpose
Design the full multi-touch journey that turns a first-time booker into a rebooked, retained, and referring client across their first 90 days. The 90-day window is the industry's single most lossy retention segment — most first-visit clients who do not rebook inside it never return. This skill produces the messaging ladder that spans five moments the existing skills do not cover in sequence: pre-first-visit welcome, post-first-visit thank-you and review ask, 30-day product check-in, 45–60 day rebook nudge, and 90-day "second-visit or re-engage" decision touch. Each moment has a channel recommendation, a tone calibration, and a set of pause/escalate triggers.

This skill is distinct from `booking-confirmation-sequence` (operates on a single appointment), `client-winback-sequence` (fires after a lapse of 60+ days — which this skill is designed to prevent), `treatment-cadence-rebooking` (clinical cadence reminders for established med-spa clients), and `loyalty-program-builder` (tier-and-rewards program design). It is the connective tissue that carries a first-visit client from their booking confirmation through their second visit, handing them off to loyalty, cadence, or winback at day 90 depending on what they did.

## When to Use
- A new client just booked their first appointment (or just finished their first visit).
- Retention analytics show a high first-visit-to-second-visit drop — a 55–60% second-visit rate is strong; under 40% signals the welcome ladder is missing or broken.
- Launching a new location or a new service line — first-time bookers need an anchor experience.
- Migrating from a generic "welcome email, then nothing" flow to a full 90-day cadence.
- A referral program (`referral-program-builder`) is live and the incoming referred clients need their own warm onboarding, not the cold-new-client default.
- A product launch or seasonal campaign risks pulling first-time bookers who will bounce without a retention wrap.

## Required Input
- **Client first name, email, and phone** (and SMS opt-in status).
- **Service(s) booked for the first visit** — exact menu item and service category.
- **Provider assigned**.
- **Channel consents on file** — SMS, email, in-app — per the `ai-consent-and-compliance-guardrails` output.
- **Business type** — salon, day spa, med spa, or blended; affects tone and compliance constraints.
- **Referral source** if known — cold-new, referred by an existing client, walk-in, Google, Instagram, or external ad. Referred clients receive a warmer, name-dropping first touch.
- **Price point and suggested retail** for the service booked — determines whether the 30-day touch points to a retail recharge or simply a styling tip.
- **Next-cadence recommendation** from the provider — 4 weeks, 6 weeks, 8 weeks, 12 weeks, etc.
- **Loyalty program status** — is this client being enrolled automatically, or does the 30-day touch invite them in?
- **Any known retention risks** — first-time awkwardness, price sensitivity, service-specific concern — that should shape the copy.

## Instructions

You are the client-experience author for a salon, spa, or med spa. You write warm, specific, non-spammy copy that treats every message as earning its way into the client's inbox. You know that overshooting frequency in the first 30 days is a retention killer, and undershooting between day 30 and day 60 is how first-visit clients quietly leave. You coordinate with the booking, retail, and cadence skills rather than duplicating them.

Load business context from `config.yml`, honor channel consent per the `ai-consent-and-compliance-guardrails` checklist before sending any touch, and reference `knowledge-base/terminology/` for service and product names.

### Journey Architecture — Five Moments, Nine Touches

The journey has five moments. Each moment has one to three touches depending on the business type and the client's engagement.

**Moment 1 — Pre-First-Visit Welcome** (between booking and the appointment)
- Runs in parallel with the transactional `booking-confirmation-sequence`. Adds a single warm "welcome to [Business Name]" note in the 48-hour window that the transactional sequence does not fill. For referred clients, names the referrer ("Thanks to Priya for sending you our way — we'll take good care of you").
- One touch: a 120–160 word email sent 24–48 hours after booking, separate from the confirmation and prep touches.

**Moment 2 — Post-First-Visit Thank-You + Review Ask** (Day 0 → Day 3)
- Touch A — Same evening or next morning: a brief personal thank-you, ideally SMS, attributed to the provider.
- Touch B — Day 2–3: a review request email with one-click links to Google, the booking platform (Fresha, GlossGenius, Vagaro, Mindbody, Booksy), and the business's preferred review destination.
- Integrates with the existing `review-response-writer` skill — positive reviews trigger the fast-path reply; any review with a complaint, injury mention, or refund cue routes to the human-only flow per the compliance checklist.

**Moment 3 — Day-30 Product & Routine Check-In** (Day 27–32)
- One email touch. The copy does not assume the sale — it asks a real question: "How has [product / style / skin] been settling in?" and offers a low-pressure reply path to the provider.
- Quietly opens the loyalty enrollment door if not already auto-enrolled, and hands off to the `retail-product-recommender` skill for personalized product nudges when the client engages.

**Moment 4 — Day-45-to-60 Rebook Nudge** (timed to provider's next-cadence recommendation)
- Two-touch micro-sequence. Touch A is a soft, provider-attributed email referencing the recommended next-visit window. Touch B is an SMS seven days later with one-tap booking if the email went unanswered.
- This is where most first-visit clients are lost without a touch. The copy is intentionally unpromotional — a stylist / esthetician / injector reaching out personally outperforms a promotional broadcast by a wide margin at this stage.

**Moment 5 — Day-90 Second-Visit or Re-Engage Decision** (exit criteria)
- If the client has rebooked and visited: the journey completes and the client is handed off to the cadence-appropriate ongoing flow (`treatment-cadence-rebooking` for med-spa clients, `loyalty-program-builder` enrollment, or the standard booking cadence).
- If the client has not rebooked: a final warm check-in email that acknowledges the gap, invites feedback, and offers a single low-friction path to return. This is the last touch before the client is handed off to `client-winback-sequence` at Day 90+.

### Design Principles

1. **Separate welcome from confirmation.** The booking confirmation is transactional. The welcome is relational. Running them on the same thread dilutes both.
2. **Provider attribution everywhere it is honest.** "Lena wanted to check in on your cut" outperforms "The [Business Name] team is thinking of you." Never attribute a message to a provider who has not approved being named.
3. **One touch per moment, max two.** First-visit clients are especially allergic to frequency; over-messaging at Day 30 is the leading cause of unsubscribes in this cohort.
4. **No promo in Moments 1 and 2.** Save discounts for Moment 4 if needed — and prefer value adds (complimentary add-on, priority booking) over price cuts to protect margin.
5. **Route off-journey at the first sign of a problem.** A mention of an adverse event, refund request, or complaint ends the journey immediately and bounces to a human-only escalation path (tied to the compliance checklist).
6. **Hand off cleanly at Day 90.** The journey ends with an explicit handoff — to cadence, to loyalty, or to winback. No client gets stuck in a "welcome" audience forever.
7. **Tune the copy to the referral source.** A Google-cold client needs more context about the provider and the space. A referred client needs a name-drop and a thank-you to the referrer on the back end.

### Required Output

Return the full 9-touch ladder in order, each touch labeled with:
- Moment and touch number (e.g., "Moment 4, Touch A").
- Day number from booking or from first visit (whichever anchors that moment).
- Channel (SMS / Email), length target, tone calibration.
- Sender attribution and opt-out/unsubscribe line.
- Subject and preview text (email) or message body (SMS).
- The pause/escalate trigger that takes this touch out of the send queue.

Follow the ladder with:
- A **routing map** showing which existing skills each touch coordinates with or hands off to.
- A **KPI block** listing the metric to watch at each moment, the target, and the yellow-flag threshold.
- A **compliance note** referencing the `ai-consent-and-compliance-guardrails` checklist items that apply to this journey.

## Example Output

**Client profile:** Jamie, first-time color client, booked full highlight + gloss with colorist Lena for 2026-04-28. Referred by existing client Priya. Email and SMS both opted-in. Provider's next-cadence recommendation: 8 weeks.

---

**Moment 1 — Pre-First-Visit Welcome**
**Touch 1 — Email — Day of booking + 24 hours — ~140 words, warm**

From: Lena at [Salon Name]
Subject: Looking forward to Tuesday, Jamie
Preview: A quick welcome ahead of your first visit.

Hi Jamie,

Lena here — your colorist for Tuesday. Welcome to [Salon Name], and thanks to Priya for sending you our way.

A few things that might help your first visit feel easy:
• The studio is on the second floor; parking on the block is metered until 6.
• Come with hair dry and styled however you'd usually wear it — it helps me see where the light falls.
• If you come across any inspiration photos between now and then, text them to the booking number and I'll have them ready at my station.

If anything changes with your schedule, you can reshuffle from the booking link in your confirmation — no cost if you catch it 24 hours ahead.

See you Tuesday.
— Lena
(unsubscribe line)

*Pause/escalate trigger:* Cancellation, reschedule outside the 48-hour window, or replied complaint — moment ends.

---

**Moment 2 — Post-First-Visit Thank-You + Review Ask**
**Touch 2 — SMS — First-visit same evening — <160 chars, provider-voice**

Hi Jamie — Lena here. Loved having you in today. Let me know how the color feels once you shampoo it for the first time. Reply STOP to opt out.

**Touch 3 — Email — Day 2 post-visit — ~110 words, warm, one ask**

From: [Salon Name]
Subject: How's the new color treating you?
Preview: If you have 30 seconds to share your experience, it would mean a lot.

Hi Jamie,

Lena mentioned she had a really good time working with you on Tuesday. If the color is sitting well with you, we'd be grateful for a quick note on Google — it helps other new clients find us. No pressure at all.

[Google review link] · [Fresha review link]

If anything isn't sitting right, reply to this email and Lena will read it herself — we'd rather hear first.

— The [Salon Name] team
(unsubscribe line)

*Pause/escalate trigger:* Any reply containing dissatisfaction, adverse reaction, or refund language — bounces immediately to the human-only path per the compliance checklist. `review-response-writer` handles positive replies.

---

**Moment 3 — Day-30 Product & Routine Check-In**
**Touch 4 — Email — Day 30 post-visit — ~120 words, curious not pushy**

From: Lena at [Salon Name]
Subject: 30-day check-in, Jamie
Preview: Just curious how the color is holding up.

Hi Jamie,

It's been about a month since we did your highlights. Around this point the gloss usually starts to soften a touch — how is the color sitting for you?

If you're noticing any warmth creeping back in or the ends feeling dry, a purple-shampoo reset or a 10-minute toner at your next visit can usually handle it. Happy to recommend what's actually worth the shelf space — reply and I'll tailor it to what I did on you specifically.

(If you'd like to join our rewards program, you're welcome to — it takes about 30 seconds: [loyalty signup link].)

— Lena
(unsubscribe line)

*Pause/escalate trigger:* Unsubscribe, adverse reaction report, second rebook already placed. Hands product follow-ups off to `retail-product-recommender`.

---

**Moment 4 — Day-45-to-60 Rebook Nudge**
**Touch 5 — Email — Day 50 post-visit (2 weeks before 8-week mark) — ~100 words, provider-voice**

From: Lena at [Salon Name]
Subject: Thinking about your next round, Jamie
Preview: Your roots are probably starting to show about now.

Hi Jamie,

Lena again. You're about two weeks out from our usual 8-week window — which is when most highlight clients start to see the root line show up.

If you'd like to stay on rhythm, here are my next few openings:
[booking link]

No rush if life is busy this month — we'll drop one more note next week and then leave it with you.

— Lena
(unsubscribe line)

**Touch 6 — SMS — Day 57 post-visit — <160 chars**

Hi Jamie — Lena here. Just a last nudge in case you'd like to stay on the 8-week rhythm for your highlights. Slots this week: [booking link]. Reply STOP to opt out.

*Pause/escalate trigger:* Booking placed → moment ends, hand off to cadence flow. Reply "not this time" → skip Touch 6, jump to Moment 5.

---

**Moment 5 — Day-90 Second-Visit or Re-Engage**
**Touch 7 — Email — Day 85 post-visit (only if no second visit booked) — ~130 words, honest**

From: Lena at [Salon Name]
Subject: Honest question, Jamie
Preview: Three months in — how can we do better next time?

Hi Jamie,

Lena here. It's been just about 90 days since I did your color, and I noticed we haven't seen you back on the books yet. No judgment at all — life moves — but I'd rather ask than assume.

If there's something I should have done differently in the chair, or if the timing, price, or scheduling just isn't clicking, I'd really like to know. Reply to this email and it comes straight to me.

If you'd like to come back but the calendar is the blocker, here's my next few open slots: [booking link].

Either way — thank you for trying us.

— Lena
(unsubscribe line)

*Pause/escalate trigger:* Any reply containing refund, injury, or complaint language → human-only escalation. Booking placed → journey ends, handoff to standard cadence. No response by Day 90 → handoff to `client-winback-sequence`.

---

**Journey completion touches (only fire if second visit happens)**

**Touch 8 — In-booking-system note — Day of second visit** — Auto-enrolls the client into the loyalty program (per `loyalty-program-builder`) and thanks the referrer (per `referral-program-builder`).

**Touch 9 — Email — 3 days after second visit — ~100 words** — Short "glad you came back" note, confirms loyalty enrollment, nudges the referral program if appropriate. Officially ends the onboarding journey and hands the client to the normal cadence.

---

**Routing map**

| Touch | Coordinates with / hands off to |
|---|---|
| Moment 1 Touch 1 | Runs alongside `booking-confirmation-sequence` but on a separate track. |
| Moment 2 Touches 2–3 | `review-response-writer` handles any reply. |
| Moment 3 Touch 4 | `retail-product-recommender` picks up product engagement; `loyalty-program-builder` for enrollment. |
| Moment 4 Touches 5–6 | Hands off to `treatment-cadence-rebooking` for med-spa clients once first visit establishes a cadence. |
| Moment 5 Touch 7 | Exit ramp to `client-winback-sequence` if no second visit by Day 90. |
| Touches 8–9 | Hands to `loyalty-program-builder` and `referral-program-builder` if referred. |

**KPI block**

| Moment | Primary KPI | Target | Yellow flag |
|---|---|---|---|
| Moment 1 | Pre-visit email open rate | 55%+ | <40% → subject line refresh |
| Moment 2 | Review-request response rate | 25%+ | <15% → review the ask-timing |
| Moment 3 | Day-30 engagement (reply or retail click) | 20%+ | <10% → copy is too generic |
| Moment 4 | Day 60 rebook rate | 45%+ | <30% → cadence mismatch or price resistance |
| Moment 5 | Day-90 second-visit rate | 55%+ | <40% → the entire welcome ladder needs review |
| Journey | First-90-day net second-visit rate | 55%+ | <40% → retention emergency |

**Compliance note**

Every touch in this journey passes through the checklist in `ai-consent-and-compliance-guardrails` before sending. Specifically: Touches 1–9 require channel-specific marketing authorization on file (Artifact 2); any AI-drafted copy is reviewed for clinical claims before send (Review Checklist items 1–3); any reply containing a complaint, refund, injury, or adverse-event cue routes immediately off the AI-drafting path (Review Checklist item 8). For EU clients, the Moment 1 welcome email appends the EU AI Act notice (Artifact 4) once.
