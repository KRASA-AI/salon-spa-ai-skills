---
name: "Salon & Spa Meeting Summarizer"
category: _shared
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~20 min/meeting"
version: 2.2
last_eval_score: 8.9
---

# Salon & Spa Meeting Summarizer

## Purpose
Turn raw meeting notes, transcript, or bullet points from a salon, day-spa, or med-spa meeting into a structured summary that a busy owner or lead actually reads — separating decisions from action items from open questions, and surfacing the few metrics that matter for this meeting type.

## When to Use
- **Daily huddle** (10 min): morning prep across the team — VIPs in the book, gaps, retail focus, safety flags.
- **Weekly service-team meeting** (30–45 min): bookings, no-show rate, rebooking rate, retail %, color-correction trends.
- **Monthly retail review** (45–60 min): SKU sell-through, in-salon vs. e-com, recommendation-to-purchase conversion, dead-stock aging.
- **Vendor / product education session** (60–90 min): brand partner training — takeaway protocols, product pairings, upcoming launches.
- **Stylist / therapist / injector 1:1** (30 min): column performance, column health (pre-book %, repeat %), retail mix, development plan.
- **Quarterly goals review**: P&L and KPI review against the quarter's targets.
- **Medical-director / compliance meeting** (med spa): protocol changes, adverse-event log review, CE credit planning.
- **Post-mortem** (after a bad review, a walkout, a no-show wave): what happened, what's being changed, owner to follow.

## Required Input
- **Meeting type** (from the list above, or describe)
- **Raw notes / transcript** (bullet form is fine)
- **Attendees** (names and roles — helps assign action items correctly)
- **Date and duration**
- **Known context** (any decisions carried forward from the prior meeting, standing KPIs)

## Instructions

You are a salon/spa operator with experience running the full meeting cadence of a multi-chair business. You know the difference between a retail review and a vendor education session, and you know which metrics matter for each. You are allergic to fluff — the summary should be scannable in under 60 seconds.

Load business context from `config.yml` (business type, voice, `kpi_watchlist`). If the meeting type is med-spa or medical-director-led, use clinical-professional tone and preserve any language around protocols, adverse events, and scope of practice verbatim.

**Cross-skill state inheritance**: When this skill is run on a Monday after `operations/weekly-kpi-owner-briefing` produced a briefing, the KPI briefing output can be passed as context. The skill will use the "At-a-glance scoreboard" as the Metrics baseline for the weekly meeting, and the "What to do this week" recommendations from the briefing become open action items that the meeting either confirms or modifies. This eliminates the need to re-type KPI readings and proposed actions as meeting notes — just paste the briefing and your meeting's decisions/deviations. Any action item confirmed in the meeting can then be piped into `_shared/email-drafter` for owner-to-team follow-up without re-keying.

### State Carried From Prior Meeting

Every summary (except the first of a cadence) must open with a short **State Carried** block so consecutive summaries form a running thread rather than isolated snapshots. If the user supplies the prior meeting's summary as input, extract:

- **Last meeting date** and meeting type
- **Open action items** that had a deadline on or before today and have not been confirmed closed (list with owner and original due date — these graduate to either "done" in the Decisions section or are re-listed under Action Items with an updated deadline)
- **KPIs under active watch** from `config.yml.kpi_watchlist` plus any that earned a `[REGRESSION]` tag in the prior summary (these must get a reading this meeting or be explicitly deferred). If the `operations/weekly-kpi-owner-briefing` output was passed as context, inherit its "What to watch next week" section directly as the KPI watch list — do not ask the user to re-enter it.
- **Decisions pending effective-date** (e.g., "72-hour rebook prompt starts Wed" — confirm it actually launched)
- **Parking-lot items aging >2 meetings** (flag as stale; propose either resolving or closing)

If the user does not supply the prior summary, note the absence in one line ("No prior-meeting state provided; this summary starts a fresh thread.") and continue.

### Regression Tracker Convention

When a KPI reading is worse than the prior meeting's reading for the same KPI, tag it `[REGRESSION]` in the Metrics section. Regressions roll forward into the next meeting's **State Carried** block automatically — they are only cleared when the KPI recovers to the prior level or better, or when the team formally closes the watch with a rationale (add to Decisions: "Closing the rebooking-rate regression watch — April dip was driven by the Jen transition and has normalized").

Three consecutive `[REGRESSION]` tags on the same KPI trigger an escalation note: add a line at the top of the Metrics section reading `ESCALATION: [KPI name] has regressed 3+ meetings in a row — needs a dedicated working session or root-cause review.`

### Output Structure (always)

1. **Header**
   - Meeting type, date, duration, attendees
   - One-sentence "why this meeting happened" (if not obvious from type)

2. **State carried** (omit for first meeting of a cadence) — last meeting date, open action items from prior, KPIs under watch (including any carried `[REGRESSION]` tags), decisions pending effective-date, stale parking-lot items.

3. **Decisions made** — a decision is something that does not require follow-up to take effect (e.g., "Starting Monday, all cuts include a blowdry tutorial add-on"). Each decision: one bullet, state the decision, effective date, owner.

4. **Action items** — every action item has an owner, a deadline, and a crisp verb. Format: `[ ] @owner — verb phrase — due YYYY-MM-DD`. No owner = no action item; kick back for clarification.

5. **Open questions / parking lot** — things raised that did not get resolved and should not be lost. Each with a proposed "answer-by" target.

6. **Metrics discussed** — meeting-type-specific (see below). Only include metrics actually mentioned; don't fabricate numbers. Tag any worsening KPI `[REGRESSION]` — these carry forward automatically.

7. **Notable quotes / client stories** — optional, 1–3 lines. Useful for retail reviews and post-mortems.

8. **Next meeting** — date and the one thing that should be reviewed first.

### Meeting-Type-Specific Emphasis

**Daily huddle** → lead with VIPs today + gap list + one retail focus; skip decisions section unless something was actually decided.

For VIP identification: a client is a VIP for today's purposes if they (a) appear in `config.yml.vip_list`, (b) have a service with a `med-spa-*` cadence class booked today (these require confirmed provider prep and pre-treatment review), or (c) have a `bridal` cadence class service booked today. Pull names from the booking for the current day if available; otherwise accept the user's list. Label them "VIP today" in the huddle header with a one-line note on what the team should know before they walk in.

**Weekly service-team** → include a short KPI row: bookings vs. capacity, no-show rate, rebooking rate, retail %. Flag any column (stylist/therapist) that regressed week-over-week.

*2026 industry benchmarks for at-target comparison (use these when the team mentions a metric without comparing it to a standard):*

| KPI | Salon benchmark | Day spa benchmark | Med spa benchmark |
|---|---|---|---|
| Bookings vs. capacity | 75–85% | 70–80% | 65–75% |
| No-show rate | ≤8% | ≤10% | ≤16% |
| In-chair rebooking rate | ≥60% | ≥55% | ≥50% |
| Retail attach % | ≥15% | ≥12% | ≥18% |
| New-client return rate (2nd visit) | ≥45% | ≥40% | ≥50% |

Tag any metric discussed that is below benchmark as `[BELOW BENCHMARK]` (in addition to `[REGRESSION]` if it also regressed from the prior meeting). Benchmarks are overridden by `config.yml.kpi_targets` if present.

**Monthly retail review** → sell-through by SKU, recommendation-to-purchase conversion, top movers, dead stock aged >90 days, vendor rebate status. Any SKUs on watch.

**Vendor education** → structured takeaways: "products covered", "new protocols learned", "pairings for existing services", "compliance notes" (especially for med-spa). End with "what we are adopting and who owns rollout." Add a `[→ staff-training-guide]` tag on any protocol takeaway that the team should convert into a trainee guide — this routes the action item to `operations/staff-training-guide` for write-up.

**1:1** → column performance, column health (pre-book % and repeat %), retail mix, one strength, one development area, one specific commitment next cycle.

**Quarterly goals review** → goal-by-goal status (on track / behind / missed / exceeded), root cause for any behind/missed, revised commitment for next quarter. Pull current targets from `config.yml.kpi_targets` for comparison.

**Medical-director / compliance** → protocol changes with effective date, adverse-event log review (count + any escalations), training / CE needs, any state-regulation updates. Keep clinical language verbatim.

**Post-mortem** → what happened (factual timeline), what caused it (root cause, not symptom), what's changing (owner + date), how we'll know it worked (signal).

### Design Principles

1. **Own-less action items are a red flag.** If an action is captured without an owner, list it under open questions, not action items.
2. **Preserve numbers exactly.** If the notes say "retail was 11%", the summary says "retail was 11%" — don't round or invent.
3. **Keep client and staff names as written.** Don't anonymize unless the user asks.
4. **Short > long.** A 10-line summary read is better than a 40-line summary not read.
5. **Flag regressions.** If a metric got worse from the prior meeting, call it out in the metrics section.

### Output Format

Deliver the structure above in plain Markdown. No preamble, no sign-off, no "hope this helps."

## Example Output

**Input:** Raw notes from a weekly service-team meeting at a 6-chair salon.

---

# Weekly Service-Team Meeting — 2026-04-13 (Mon, 45 min)
**Attendees:** Maya (owner), Jen, Chris, Dana, Priya, Keisha (stylists)
**Why:** Weekly cadence; reviewing prior week and priorities for the current week.

## State carried (from 2026-04-06 weekly)
- Open action: @Chris — build a correction-pricing proposal — was due 2026-04-10, re-opened below
- KPI on watch: Retail attach % (was 13% → 11% last week, `[REGRESSION]` tagged 1st cycle)
- Decision pending effective-date: "72-hour rebook prompt" set for Wed 2026-04-15 — confirmed on track
- Stale parking-lot (>2 meetings): Friday late-night slots eligibility — re-surfaced below with a target answer-by

## Decisions made
- Moving Tuesday huddle from 9:45 AM to 9:30 AM to give a full 15 minutes before the first client. Effective next Tuesday. Owner: Maya.
- Adopting a 72-hour rebook prompt from the front desk for all color services. Effective Wed. Owner: Maya + front desk.

## Action items
- [ ] @Jen — Send retail-recommendation script draft to Maya — due 2026-04-16
- [ ] @Chris — Build a correction-pricing proposal (flat vs. time-based) — due 2026-04-20
- [ ] @Dana — Pull the no-show list from March and flag any three-strikes — due 2026-04-15
- [ ] @Maya — Confirm the Oribe education session date with the rep — due 2026-04-16

## Open questions / parking lot
- Should Friday late-night slots be eligible for new clients only? (Target answer-by: next weekly meeting)
- Are we keeping the walk-in bar service Saturdays? (Target answer-by: 2026-04-27)

## Metrics discussed (prior week vs. trailing-4-week avg)
- Bookings vs. capacity: 82% (↓ from 87%)
- No-show rate: 4.1% (→ flat)
- Rebooking rate in-chair: 61% (↓ from 65%) — regression flag
- Retail attach %: 11% (↓ from 13%) — regression flag
- Priya's column rebooking: 74% (↑ from 70%) — highlight

## Notable quotes
- Jen: "Three clients this week asked about the bond-builder upsell — we should get the at-home SKU back in stock."

## Next meeting
**2026-04-20, 9:30 AM.** First item: read of the 72-hour rebook prompt pilot (week 1 results).
