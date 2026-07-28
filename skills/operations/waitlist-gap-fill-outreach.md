---
name: Waitlist Gap-Fill Outreach Generator
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~10 min per gap"
version: 1.1
last_eval_score: 9.0
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

**Ask only for what config cannot supply.** Everything in the Config Integration table below (business name, provider roster, service names, brand voice, booking link, opt-out language, state) comes from `config.yml` when it is populated — **do not ask the user for it.** A front desk with a chair emptying in 90 minutes should be asked for the opening and the waitlist, and nothing else. The list below is what a run genuinely needs from a human; the italicized items are config-backed and should only be requested if config is absent or stale.

- **Opening details**: service type, duration, provider, date, exact time window (e.g., "Balayage, 3 hr, Jen, Tue 4/14, 1:00–4:00 PM")
- **Waitlist entries**: names, requested services, providers they'll see, service-level preferences (e.g., Level 1 stylist only, female therapist only), time-window flexibility (e.g., "weekday afternoons only")
- *(config-backed)* **Business context**: salon / spa / med spa, brand voice, typical opt-in channel (SMS is most common)
- **Urgency tier**: hot (under 4 hours to fill), warm (4–24 hours), cool (1–3 days)
- **Offer stance**: none, small add-on incentive (free scalp massage, complimentary blowout, LED add-on), or priority-perk ("book this and you keep your waitlist priority for next month")
- **Compliance flags**: TCPA opt-in status per client, no-contact list, state medical-marketing restrictions for med-spa services
- *(config-backed)* **Claim mechanism**: reply-to-claim SMS, one-tap booking link, phone call-back, provider-DM

## Instructions

You are an operations-side retention specialist who has run front desks for salons and med spas. You understand that a wasted hour at peak rate is pure lost margin, and that targeted, fast, provider-attributed outreach fills a short-notice gap far more reliably than a generic "we've got a spot" blast. (Do not quote a recovery-rate figure at the practice. Fill rate depends on waitlist depth, service type, and notice window — measure the practice's own baseline in the tracking block below and improve against that, rather than citing an assumed industry number.)

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service and provider naming. Never invent client preferences or availability.

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Sign-off on every SMS and email; the "it's [salon]" identifier in the opening line (a gap-fill text from an unnamed sender reads as spam and gets ignored) | `[Practice Name]` — flag as **required before send**; never dispatch a message with an unresolved name token |
| `business.business_type` | Salon vs. day spa vs. med spa — gates the clinical rules: a med-spa gap-fill must not name a clinical service in an external SMS (PHI), must not imply the treatment is pre-approved, and must respect the adverse-event / service-recovery suppression flags below | if any `med-spa-*` cadence class appears in `services.cadence_class`, apply the med-spa rules to those services only; otherwise apply salon defaults and flag the assumption |
| `business.location.state` | Fires the state gates: **CA SB 351** (a gap-fill may *book* a clinical-tier slot but must never imply the treatment is pre-approved — the consultation/GFE gate survives into the offer), **CA AB-489** (no clinical titles or credentialing language if an AI agent delivers the message), and the state's **call-recording consent** regime if the gap-fill is delivered by an AI voice agent rather than text | ask once; do **not** guess — the clinical-copy and voice gates turn on it. Flag the gap in the packet header |
| `staff.roster` | Provider-first framing (Design Principle 4): the message says "Jen has an opening," with the provider's real name and, for med spas, their correct credential — not "the salon has an opening" | `[Provider]` — flag as required; provider-first framing is the skill's core lever and degrades badly without a real name |
| `services.menu` + `services.cadence_class` | Correct service naming in the offer, the duration/tier fit in the Matching Logic ranking, and the gap-filler tier | infer from the opening description; flag the naming assumption |
| `tools.booking_link` | The one-tap claim link (per-provider deep link where the platform supports it — Boulevard, Mindbody, Zenoti, Vagaro, GlossGenius all do) | fall back to reply-to-claim SMS and note that the claim mechanism is degraded — a reply-to-claim needs a human watching the inbox inside the urgency window |
| `tools.pos` / booking platform | Whether the urgency-tier hold can actually be enforced (a 90-minute claim hold is a promise the platform has to keep) and whether waitlist status is machine-readable | state plainly that the hold is manual and must be watched by the front desk |
| `compliance.opt_out_language` (per channel) | The SMS opt-out line and the email unsubscribe footer, in the exact wording the practice already uses | `Reply STOP to opt out` (SMS) / standard unsubscribe (email) — flag for confirmation |
| `business.voice.tone` | Warmth and register of the copy (warm-casual, polished-clinical, luxe-minimal) | warm-professional; surface the assumption inline |

**Never ship a bracketed stub as final copy.** `[Salon]`, `[booking link]`, `[First Name]` and the like are config-resolution tokens, not send-ready text — a gap-fill message that goes out with `[Salon]` in it is worse than no message at all. If a key is missing, produce the packet, leave the token visible, and put a hard flag at the top of the packet naming exactly which values the front desk must fill before the first send.

### Delivery-Channel Gate — If an AI Agent Sends This

This skill owns the **offer**. If the message is delivered by an AI voice agent or an AI text bot rather than by a human at the front desk, `operations/ai-receptionist-disclosure-and-escalation` owns the **opening seconds** of that contact, and its outbound block is a hard dependency:

- Gap-fill outreach is **outbound**, which is the highest-exposure category. US telephone-consumer rules already treat an AI-generated voice as an artificial voice, so an AI voice gap-fill call needs prior express consent — and the practice's opt-in language should **name AI voice specifically** before an outbound voice agent is switched on. A consent form that says only "you may receive automated messages" may not carry the load.
- The AI disclosure and a working opt-out belong in the **opening seconds**, not appended at the end of the offer.
- Suppression logic must distinguish marketing opt-outs from transactional contact. A gap-fill offer is **marketing**, not a transactional appointment confirmation — a client who opted out of marketing does not get a gap-fill text.
- Human-delivered SMS/email from the front desk does not fire this gate. It fires only when a bot places the call or sends the text.

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

*(Config-resolved throughout: `business.name` = Copper & Sage Salon; `staff.roster` → Jen Marek, senior colorist; `tools.booking_link` = copperandsage.com/book/jen; `compliance.opt_out_language.sms` = "Reply STOP to opt out". These are the practice's real values, not stubs — that is the point. Swap in your own.)*

**Opening summary**
Balayage + toner, 3 hr, Jen (senior colorist), Tue 2026-04-14, 1:00–4:00 PM. Urgency: warm (23 hours to fill — sending 2:00 PM Monday). Offer: complimentary glossing add-on. Delivery: front-desk SMS/email (human-sent — the AI-agent outbound gate does not fire).

*Timing, made explicit because a warm-tier gap-fill fails on timing more often than on copy:* all five messages go out **together at 2:00 PM Monday**, each carrying the same **90-minute** claim window (the Warm-tier spec). Windows close at **3:30 PM**. Only then does the broadcast fire. Staggering the sends, or letting a claim link outlive its stated window, is how two clients end up believing they hold the same chair.

**Ranked candidates**

1. Maya R. — exact-fit (balayage with Jen, weekday afternoons) — last visit 2025-12-02, highlights with Jen — SMS opt-in — no flags
2. Priya S. — service + provider (balayage with Jen, time window is mornings) — last visit 2026-01-20 — SMS opt-in
3. Dana L. — service match, different provider (wants balayage, flexible on stylist) — last visit 2026-02-11 — email opt-in, SMS opted out
4. Keisha B. — provider match (wants a cut with Jen; could tuck into a shorter gap) — last visit 2026-03-22 — SMS opt-in
5. Isabel T. — level-match — last visit 2025-10-04 — SMS opt-in — contacted about a gap 3 days ago (**outside the 48-hour suppression window, so she is eligible**); this would be her **second** consecutive unanswered gap-fill, which trips the Design Principle 5 pause *after* this send, not before it

**Messages**

*To Maya — SMS*
Hi Maya, it's Copper & Sage — Jen just had a 1:00 PM balayage slot open up tomorrow (Tue 4/14). Yours if you want it, plus a complimentary gloss. Reply YES in the next 90 min to claim. Reply STOP to opt out.

*To Priya — SMS*
Hi Priya, it's Copper & Sage — Jen has a balayage opening tomorrow at 1:00 PM. It's outside your usual morning window, but wanted to flag it in case it works — complimentary gloss if you claim. Reply YES within 90 min. Reply STOP to opt out.

*To Dana — Email*
Subject: A balayage opening with Jen — tomorrow at 1:00 PM
Preview: Complimentary gloss if it works for you.

Hi Dana,

We had a last-minute opening on Jen's book tomorrow (Tuesday 4/14) from 1:00 to 4:00 PM — balayage and toner, plus a complimentary gloss. You're on the balayage waitlist, so I wanted to check with you before we widened the broadcast.

If it works, claim it here: copperandsage.com/book/jen. This hold is good for 90 minutes (until 3:30 PM today) — after that we open the slot to the wider waitlist.

Warmly,
Copper & Sage Salon
*Prefer not to hear about last-minute openings? Unsubscribe here and we'll only send your appointment confirmations.*

*To Keisha — SMS*
Hi Keisha, it's Copper & Sage — Jen has a 3-hour block open tomorrow 1–4 PM. Plenty of room for your cut plus a treatment or gloss if you'd like. Reply YES within 90 min to grab it. Reply STOP to opt out.

*To Isabel — SMS (send, but mark)*
Hi Isabel, it's Copper & Sage — Jen has a color block open tomorrow 1–4 PM if you'd like it. Reply YES within 90 min. Reply STOP to opt out.
→ **Tracking note:** if this one goes unanswered too, that is two consecutive ignored gap-fills — pause her waitlist outreach and trigger a front-desk check-in (Design Principle 5). *The rules suppress on a **48-hour** re-contact window and on **two consecutive** non-responses; three days and one non-response clears both, so she is contacted. Do not invent a stricter rule than the skill states — silently over-suppressing shrinks the list that fills your chair.*

**Broadcast fallback** (send only if all five candidates above pass — i.e. no YES by 3:30 PM, once the 90-minute claim windows from the 2:00 PM send have all expired)
Hi Sam, it's Copper & Sage. Jen has a color opening tomorrow 1–4 PM. Complimentary gloss included. First to reply YES gets it. Reply STOP to opt out.

*(The broadcast is mail-merged on first name from the waitlist — it is still personalized on the one token that matters. A merge field that fails open and sends "Hi [First Name]" is the single most common way a gap-fill blast reads as spam; test the merge on yourself before the broadcast goes out.)*

**Tracking block**
- On claim: log candidate name, channel, match tier, minutes-to-claim.
- On pass / silence: log expiry; if 2 consecutive gap-fills ignored, move client to "pause waitlist outreach" list and trigger front-desk task.
- Weekly KPI review (route to `operations/weekly-kpi-owner-briefing`): fill rate by urgency tier, median minutes-to-claim, and share of fills coming from the top-3-ranked candidates. **Set the target from your own first four weeks of data, not from an industry figure** — the useful question is whether ranked outreach beats your broadcast baseline, and by how much. If most fills come from the broadcast rather than the ranked list, the Matching Logic inputs (stated windows, provider preferences) are stale and need a front-desk refresh.

## Version History
- **1.1** (2026-07-13) — Added the **Config Integration table** (9 keys with per-key fallbacks) and the hard "never ship a bracketed stub as final copy" rule; the skill was the last in the library still leaning entirely on manual input for business context. Fully resolved the worked example (Copper & Sage Salon / Jen Marek / real claim link / real opt-out lines) so it demonstrates send-ready copy rather than templated stubs. Added the **Delivery-Channel Gate** — when an AI voice or text agent (not a human at the front desk) delivers the gap-fill, `operations/ai-receptionist-disclosure-and-escalation`'s outbound block governs the opening seconds, AI-voice consent must be named in the opt-in, and marketing-vs-transactional suppression must be honored; gap-fill is the outbound use the regulations KB flags as highest-exposure. Added the CA SB 351 / AB-489 state gates to the config table, and a note reconciling Required Input against config so the skill stops asking for what config already supplies.
  - **Removed, deliberately** (this edit was corrective, not purely additive — saying "additive, nothing removed" when something *was* removed is the same species of false claim this cycle spent its time deleting): the unsourced *"the best waitlist tools recover 60–70% of last-minute cancellations"* and the invented *"target ≥ 60% of fills from top-3-ranked candidates"* KPI. Both replaced with measure-your-own-baseline guidance.
  - **Fixed in the worked example**, where the copy contradicted the skill's own rules: Isabel was being skipped for a non-response 3 days old, but the stated rules suppress only inside a **48-hour** re-contact window and only after **two consecutive** non-responses — she was eligible, and silently over-suppressing shrinks the list that fills the chair. Also unified the claim windows (all five sends at 2:00 PM Monday, all 90-minute holds, broadcast at 3:30 PM); the prior example gave the SMS recipients 90 minutes but held the email until 9:00 AM the next day, leaving the chair unoffered in the ~17-hour gap between. The send time is set so the stated "23 hours to fill" actually lands inside the Warm band (4–24 h) the example claims — an example whose own numbers put it in a different urgency tier than the one it says it's in teaches the wrong pattern.
- **1.0** — Initial release.
