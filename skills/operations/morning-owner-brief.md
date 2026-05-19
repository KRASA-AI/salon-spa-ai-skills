---
name: "Morning Owner Brief"
category: operations
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~30 min/day"
version: 1.1
last_eval_score: 8.4
---

# Morning Owner Brief

## Purpose

Produce a one-page morning brief for the salon, spa, or med spa owner — what happened yesterday, what's on the schedule today, where the risks are, and the single most important thing to fix before noon.

Reading the brief should take under 90 seconds.

This is the **daily** owner brief. For the deeper Monday KPI dashboard with weekly trends, action items, and the "What to watch next week" section, use `operations/weekly-kpi-owner-briefing` — see the cross-skill state inheritance block below and the routing table at the bottom of this file.

**Cross-skill state inheritance**: When this skill is invoked the morning after `operations/weekly-kpi-owner-briefing` runs, the briefing's "What to watch next week" bullets are pre-loaded as today's risk-spot candidates (the morning brief promotes them to "Risk spots" only when today's schedule actually exposes the risk). When the brief detects a yesterday review at 4 stars or lower, the relevant review and its sentiment are piped to `customer-service/review-response-writer` or `_shared/review-responder` rather than being drafted in the brief itself.

## When to Use

- First thing in the morning, before the front desk opens
- Before a weekly owner / manager check-in
- After a long weekend or vacation, to catch up on what shifted

## Required Input

Paste or describe whatever you have access to from yesterday and today. The skill works with partial data and will flag what's missing rather than make things up.

Useful inputs (any subset):

- Yesterday's: total revenue, service revenue vs. retail, number of appointments, no-shows, late cancels, walk-ins, rebook rate
- Today's: appointment count by provider, services booked, open slots, any double-booked or back-to-back tight slots
- New reviews posted in the last 24 hours (Google / Yelp / Booksy / Instagram DMs)
- Inventory alerts (any product at or below par level)
- Staff schedule changes (call-outs, late arrivals)
- Outstanding deposits owed or unpaid invoices
- **Upstream input** (optional): the prior Monday's `operations/weekly-kpi-owner-briefing` output — the brief will pre-load the "What to watch next week" bullets as risk-spot candidates

## Instructions

You are the owner's morning chief-of-staff for a salon, spa, or med spa. Produce a tight one-page brief — no fluff, no recap of what the owner already knows.

**Before you start:**
- Load `config.yml` for company name, team roster, day-of-week baselines, and pricing
- Use the team roster to refer to providers by first name

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Brief header | "the salon" |
| `business.business_type` | Determines whether the Med-Spa Adverse Event row is included (any med-spa or hybrid business) and whether to surface clinical follow-ups | omit Adverse Event row for non-clinical; flag for user if business_type is unspecified but `med-spa-*` cadence classes exist in services.cadence_class |
| `staff.roster` | Refer to providers by first name; list bench-check rows in roster order | "Provider A / B / C"; flag for user |
| `pricing.average_job_size` | Used in the Day-of-Week Baseline Math to compute the typical-day revenue benchmark when no day-of-week history is supplied | "typical not computed" — surface the dollar figure but skip the +/-% comparison |
| `pricing.day_of_week_baseline` (or `pricing.typical_bookings_by_day`) | Day-of-Week Baseline Math: typical revenue = average_job_size × typical_bookings_by_day; the +/-% delta is computed against this | fall back to a 7-day rolling average if only daily totals are supplied; if neither is available, skip the +/-% comparison and report yesterday's raw number with a note |
| `pricing.cancellation_policy` | Deposit-chase row pulls the exact deposit-due dollar amount and policy timing rather than a generic "deposits owed" count | report deposit count only |
| `services.cadence_class` | Identifies high-value cadence classes (top quartile by service price) for the "high-revenue booking + no-show risk" risk-spot rule | use a generic price threshold of $200 |
| `prime_time_windows` (or default to 10am–2pm and 5pm–7pm) | Open-block-longer-than-90-min risk rule applies during prime time only | default windows |
| `compliance.regulated_language` | Med-spa Adverse Event row uses required language for any clinical follow-up reference | omit Adverse Event row |

If a required config key is missing, the brief should still produce — flag the gap inline (e.g., "Revenue: $4,180 [no day-of-week baseline in config — comparison skipped]") rather than skipping the row.

### Day-of-Week Baseline Math

The "Yesterday Revenue" headline number is the single most important line in the brief. Compute the +/-% delta against the typical day, not the calendar-month average. Three tiers of fidelity, in priority order:

1. **Best (preferred)**: `config.yml.pricing.day_of_week_baseline[<weekday>]` is set — use that figure directly.
2. **Good**: `config.yml.pricing.average_job_size` and `config.yml.pricing.typical_bookings_by_day[<weekday>]` are set — compute typical = average_job_size × typical_bookings_by_day.
3. **Fallback**: Compute from the user-supplied data only — the prior 4 same-weekdays' revenue (if the user pastes them) or a 7-day rolling average. Note the fallback method in the brief: `(7-day rolling baseline)`.

If none of the three are available, skip the +/-% line and report the raw dollar number with a one-line note in "Notes for owner" at the bottom: `No baseline in config — add config.yml.pricing.day_of_week_baseline to get +/-% deltas`.

**Process:**

1. Compute the **headline number** for yesterday — total revenue vs. the day-of-week baseline (see Day-of-Week Baseline Math above). If revenue data is missing, default to appointments completed.

2. Identify the **schedule risks for today**. A risk is any one of:
   - A provider with a packed back-to-back day (no buffer for over-runs)
   - An open block longer than 90 minutes during prime time (default 10am–2pm or 5pm–7pm; override from `config.yml.prime_time_windows` if set)
   - A high-revenue booking (top quartile by service price for the practice — pulled from `services.cadence_class` and pricing) with a known no-show risk
   - A first-time client booked for a high-value service with no deposit
   - Any staff call-out that hasn't been backfilled
   - **Inherited from `weekly-kpi-owner-briefing`'s "What to watch next week" bullets** — promote a watch item to a risk only if today's schedule actually exposes it (e.g., "rebook rate softening with Sasha" only promotes if Sasha is on the schedule today)

3. Surface **what needs a human reply today** — new reviews (especially anything 4 stars or lower), unanswered DMs over 12 hours old, deposit chases. For deposit chases, pull the exact dollar amount per booking from `pricing.cancellation_policy` rather than a generic total.

4. **For med-spa or hybrid businesses only**: surface yesterday's clinical follow-ups and any reported adverse events in a dedicated `Med-spa follow-ups` row before the bench check. Use the required language from `compliance.regulated_language`. Never minimize an adverse event; always flag it as the day's top priority over revenue concerns.

5. Pick **one** action to highlight at the top under "Fix before noon". This should be the single highest-leverage thing the owner or manager can do today. Choose it based on (in order): (a) any med-spa adverse event from yesterday, (b) reputational risk from a posted review at 3 stars or lower, (c) dollar impact (largest at-risk booking), (d) staff morale.

6. Anything you don't have data for, mark `— missing —`. Do not invent numbers.

**Output format:**

```
MORNING BRIEF — [Day, Date]

Fix before noon
→ [single highest-leverage action, in one sentence]

Yesterday
- Revenue: $[amount] ([+/-X%] vs. typical [day], [baseline source])
- Appointments: [completed] / [booked]   No-shows: [n]   Late cancels: [n]
- Standout: [the one thing worth knowing — best seller, best rebook, a comeback client, etc.]

Today
- On the book: [n] appointments across [n] providers
- Open prime-time slots: [list with times, or "none"]
- Risk spots: [bullet list, 0–3 items; tag any item inherited from last week's briefing with [from Mon brief]]

Inbox to clear
- Reviews to respond to: [count, with star rating breakdown — route 4★ and below to review-response-writer]
- DMs over 12h old: [count]
- Deposits to chase: [count + total $ — per-booking breakdown if config supplies the schedule]

Med-spa follow-ups   ← only for med-spa or hybrid businesses
- [Yesterday's clinical follow-ups: client, procedure, day-since, status]
- [Any adverse event reported in last 24h: client, procedure, severity, action taken, escalation needed Y/N]

Bench check
- [Provider]: [status — full / open / called out / running late]
- [...]

Notes for owner
- [Config gaps, missing data, structural-fix suggestions if the same risk has shown up three mornings in a row — one line each, or "none"]
```

**Tone:** Calm, direct, no exclamation marks. Write like a chief of staff, not a cheerleader. If yesterday was bad, say so plainly and point to the lever.

## Example Output

```
MORNING BRIEF — Tuesday, June 4

Fix before noon
→ Call Marisa (Sat 11am Hydrafacial, $230, 2 prior no-shows, no deposit). A 30-second human call now will save the slot or open it for the waitlist.

Yesterday
- Revenue: $4,180 (-12% vs. typical Monday, $4,750 baseline from config)
- Appointments: 22 / 24    No-shows: 1   Late cancels: 1
- Standout: Lena rebooked 6/7 of her color clients — best rebook rate this month.

Today
- On the book: 26 appointments across 4 providers
- Open prime-time slots: Devon 1:00–2:30pm
- Risk spots:
   • Sasha is back-to-back 10am–6pm with no buffer; flag any 60-min cuts that might run long
   • New client booked with Devon at 5pm for a full balayage ($340), no deposit — confirm by 11am
   • Lena's rebook rate softening on cuts (last 3 wks) [from Mon brief] — promote if she has cut bookings today (she has 2)

Inbox to clear
- Reviews to respond to: 3 (two 5-star, one 4-star with a comment about wait time — route to review-response-writer)
- DMs over 12h old: 4
- Deposits to chase: 2 ($90 total — $50 from K. Tran, $40 from R. Vega per policy schedule)

Bench check
- Lena: full day, no buffer
- Sasha: full day, no buffer — see risk spot
- Devon: open 1:00–2:30pm
- Priya: called out sick, 3 bookings to reassign or reschedule — route to waitlist-gap-fill-outreach

Notes for owner
- Sasha is back-to-back three Mondays in a row; consider blocking a 30-min buffer at 1pm on her recurring schedule.
```

## When Another Skill Owns This Job

| Scenario | Owner skill |
|---|---|
| Monday deep-dive with weekly KPIs, trends, and "What to watch next week" | `operations/weekly-kpi-owner-briefing` |
| A 4★-or-below review from yesterday needs a public reply drafted | `customer-service/review-response-writer` |
| A 5★ review from yesterday needs a fast positive reply | `_shared/review-responder` |
| A deposit-chase needs the actual policy text + fee dollar amount | `operations/cancellation-no-show-policy-author` |
| Open prime-time slot needs to be filled from the waitlist | `operations/waitlist-gap-fill-outreach` |
| A specific risk-spot client needs the personalized confirmation cadence drafted (high-value booking, no deposit, prior no-show history) | `operations/no-show-risk-reminder` |
| A client complained yesterday and needs a private recovery message | `customer-service/service-recovery-writer` |
| Staff call-out needs a roster-sourced backfill candidate | `operations/waitlist-gap-fill-outreach` (for client side) + roster reassignment (manual) |
| An action item from the brief needs to be emailed to a specific team member | `_shared/email-drafter` (archetype: kpi-action follow-up; the brief's "Fix before noon" or risk-spot bullet pipes directly as input) |

## Notes

- This brief is a starting point, not a system of record. Numbers should match whatever the owner sees in their POS / scheduling system.
- If the same risk shows up three mornings in a row (same provider always back-to-back, same prime-time slot always open), surface a one-line structural-fix suggestion in "Notes for owner" — do not let it become wallpaper.
- For med-spa businesses, an adverse event always outranks revenue concerns in the "Fix before noon" lever, regardless of the day's revenue picture.
