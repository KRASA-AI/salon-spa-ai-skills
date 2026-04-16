---
name: Waitlist Gap-Fill Outreach Generator
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min per gap"
version: 1.0
last_eval_score: null
---

# Waitlist Gap-Fill Outreach Generator

## Purpose
Turn a last-minute cancellation, no-show, or unbooked stretch into a filled chair by generating short, time-sensitive outreach to the right waitlisted clients. Produces ranked candidate lists, personalized SMS/email/push copy, a one-tap claim link, and a fallback "walk-in window" broadcast if no one claims within the urgency window.

This skill is the operational counterpart to the `client-winback-sequence` (lapsed clients) and `booking-confirmation-sequence` (pre-visit). It runs when a chair is about to sit empty and every minute matters.

## When to Use
- A client just cancelled within 24 hours of their appointment.
- A no-show leaves a stylist, therapist, or nurse injector with an open hour.
- A back-to-back cancellation leaves a 2–3 hour gap.
- A slow Tuesday morning has three stylists with empty columns.
- You want to convert standing waitlist entries into reliable short-notice rebookings rather than generic "got a spot" blasts.

## Required Input
- **Opening details**: service type, duration, provider, date, exact time window (e.g., "Balayage, 3 hr, Jen, Tue 4/14, 1:00–4:00 PM")
- **Waitlist entries**: names, requested services, providers they'll see, service-level preferences (e.g., Level 1 stylist only, female therapist only), time-window flexibility (e.g., "weekday afternoons only")
- **Business context**: salon / spa / med spa, brand voice, typical opt-in channel (SMS is most common)
- **Urgency tier**: hot (under 4 hours to fill), warm (4–24 hours), cool (1–3 days)
- **Offer stance**: none, small add-on incentive (free scalp massage, complimentary blowout, LED add-on), or priority-perk ("book this and you keep your waitlist priority for next month")
- **Compliance flags**: TCPA opt-in status per client, no-contact list, state medical-marketing restrictions for med-spa services
- **Claim mechanism**: reply-to-claim SMS, one-tap booking link, phone call-back, provider-DM

## Instructions

You are an operations-side retention specialist who has run front desks for salons and med spas. You understand that a wasted hour at peak rate is pure lost margin, and that the best waitlist tools recover 60–70% of last-minute cancellations when the outreach is targeted and quick.

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service and provider naming. Never invent client preferences or availability.

### Matching Logic

Rank waitlist candidates against the opening using this priority:

1. **Exact-fit match** — requested the same service, same provider, and the opening falls inside their stated window. Highest priority.
2. **Service + provider match, flexible on time** — same service, same provider, time outside their stated window but within 2 hours of it.
3. **Service match, different provider** — same service, different qualified provider. Include only if the client's history shows openness to switching.
4. **Provider match, different service** — e.g., stylist has a color gap, waitlist client wants a cut with the same stylist. Useful for 1–2 hour openings.
5. **Level-match / gap-filler** — service tier and duration fit the gap; provider and service are fungible. Lowest priority; use for broadcast-style fills.

Never surface a candidate if:
- They were contacted about a gap in the last 48 hours and did not respond.
- They have an existing appointment inside 24 hours of the opening (risk of cannibalization).
- They are flagged opt-out on the channel you'd use.
- Their last visit triggered a service-recovery flag (not yet resolved) or an adverse-event note (med spa).

### Outreach Tiers

Choose the outreach pattern based on the urgency tier:

**Hot (under 4 hours to fill)** — SMS or push only; 1 message; reply-to-claim within 20 minutes; if unclaimed, automatic broadcast to the next three candidates in rank order.

**Warm (4–24 hours to fill)** — SMS primary; email fallback for opted-out-of-SMS; 1 message; claim link good for 90 minutes; automatic broadcast widening if unclaimed.

**Cool (1–3 days)** — Email primary; SMS nudge for top 3 candidates; claim link good 24 hours; no broadcast widening (wait for a true shortage).

### Design Principles

1. **One message, one decision.** "Reply YES to claim" or tap-to-book. Avoid a wall of options.
2. **Name the saving.** Short-notice openings often come with perks — state them plainly ("complimentary blowout while Jen finishes your color").
3. **Respect the waitlist promise.** If a client is #1 on the list, honor that sequence; don't broadcast over them.
4. **Use provider-first framing where trust is high.** For med spa injectors and long-tenured stylists, "Jen has an unexpected opening" is stronger than "The salon has an opening."
5. **Protect the next touch.** If a client ignores 2 gap-fills in a row, pause their waitlist eligibility and prompt a front-desk check-in — don't keep burning signal.
6. **Comply with TCPA / CASL / GDPR.** Include opt-out language on SMS, honor preference timing (no SMS before 8 AM / after 9 PM local unless the client explicitly set otherwise).

### Output Structure

Produce a gap-fill packet containing:

1. **Opening summary** — 1 line, same format as input.

2. **Ranked candidate list** — top 5, each with:
   - Name and match-tier (exact-fit, service+provider, service only, etc.)
   - Last visit date and service (for context)
   - Preferred channel and time-of-day
   - Any flags (opt-out, flagged, back-to-back)

3. **Personalized messages** — one per top candidate, channel-appropriate, following the tier pattern above. Subject + preview + body for email; single message under 160 characters where feasible for SMS (or note if it will segment).

4. **Broadcast fallback** — a single, non-personalized message ready to send to the remaining qualified waitlist if the top 5 all pass. Name no specific client, keep the offer generic, and include claim mechanics.

5. **Tracking block** — what to log when a candidate claims (timestamp, channel, match tier) and what to log when unclaimed (sequence expiry, next action).

## Example Output

**Opening summary**
Balayage + toner, 3 hr, Jen (senior colorist), Tue 2026-04-14, 1:00–4:00 PM. Urgency: warm (23 hours to fill). Offer: complimentary glossing add-on.

**Ranked candidates**

1. Maya R. — exact-fit (balayage with Jen, weekday afternoons) — last visit 2025-12-02, highlights with Jen — SMS opt-in — no flags
2. Priya S. — service + provider (balayage with Jen, time window is mornings) — last visit 2026-01-20 — SMS opt-in
3. Dana L. — service match, different provider (wants balayage, flexible on stylist) — last visit 2026-02-11 — email opt-in, SMS opted out
4. Keisha B. — provider match (wants a cut with Jen; could tuck into a shorter gap) — last visit 2026-03-22 — SMS opt-in
5. Isabel T. — level-match — last visit 2025-10-04 — SMS opt-in — contacted about a gap 3 days ago, no reply (soft flag)

**Messages**

*To Maya — SMS*
Hi Maya, it's [Salon] — Jen just had a 1:00 PM balayage slot open up tomorrow (Tue 4/14). Yours if you want it, plus a complimentary gloss. Reply YES in the next 90 min to claim. STOP to opt out.

*To Priya — SMS*
Hi Priya, Jen has a balayage opening tomorrow at 1:00 PM. It's outside your usual morning window, but wanted to flag in case it works — complimentary gloss if you claim. Reply YES within 90 min. STOP to opt out.

*To Dana — Email*
Subject: A balayage opening with Jen — tomorrow at 1:00 PM
Preview: Complimentary gloss if it works for you.

Hi Dana,

We had a last-minute opening on Jen's book tomorrow (Tuesday 4/14) from 1:00 to 4:00 PM — balayage and toner, plus a complimentary gloss. You're on the balayage waitlist, so I wanted to check before we widened the broadcast.

If it works, tap here to claim: [booking link]. This hold expires at 9:00 AM tomorrow.

Warmly,
[Salon Name]

*To Keisha — SMS*
Hi Keisha, Jen has a 3-hour block open tomorrow 1–4 PM. Plenty of room for your cut plus a treatment or gloss if you'd like. Reply YES within 90 min to grab it.

*To Isabel — (skip this cycle)*
Soft flag: unresponsive to last gap-fill 3 days ago. Pause outreach until front-desk check-in.

**Broadcast fallback** (send only if top 4 all pass by 9:00 AM)
Hi [First Name], it's [Salon]. Jen has a color opening tomorrow 1–4 PM. Complimentary gloss included. First to reply YES gets it. STOP to opt out.

**Tracking block**
- On claim: log candidate name, channel, match tier, minutes-to-claim.
- On pass / silence: log expiry; if 2 consecutive gap-fills ignored, move client to "pause waitlist outreach" list and trigger front-desk task.
- Weekly KPI review: fill rate by urgency tier, median minutes-to-claim, % of fills from top-3-ranked candidates (target ≥ 60%).
