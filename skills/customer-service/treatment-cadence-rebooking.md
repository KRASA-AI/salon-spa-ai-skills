---
name: Med Spa Treatment-Cadence Rebooking Reminders
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/cadence campaign"
version: 1.0
last_eval_score: null
---

# Med Spa Treatment-Cadence Rebooking Reminders

## Purpose
Generate treatment-specific rebooking reminders timed to the clinical cadence of each aesthetic service — Botox at 10–14 weeks, filler at 6–12 months, chemical peel series at 3–6 weeks, laser hair removal at 4–6 weeks, HydraFacial at 4 weeks, CoolSculpting follow-up at 8–12 weeks. Each message ties the timing to the clinical "why" (results fading, series consolidation, seasonal window) and is attributed to the treating provider rather than the clinic, a pattern shown to lift conversion materially over generic clinic-voice outreach.

This skill is distinct from the general `client-winback-sequence` (lapsed client re-engagement) and the `booking-confirmation-sequence` (pre-visit confirmations). It sits in the middle: the client is neither lapsed nor booked — they are in their optimal retreatment window and need a clinically credible nudge.

## When to Use
- A client's last injectable, laser, energy-based, or skincare treatment is approaching its optimal retreatment date.
- Building a rebooking automation inside a med-spa CRM (Pabau, AestheticsPro, PatientNow, Zenoti Connect, etc.).
- Designing a multi-treatment "results calendar" for a client who mixes injectables, lasers, and facials.
- Recovering a package-deal client mid-series who has fallen behind schedule.
- Rebuilding a stale generic reminder flow that currently says "Time to rebook!" with no clinical context.

## Required Input
- **Client first name** (and pronoun if tracked)
- **Treating provider name and credential** (e.g., "Dr. Chen, MD", "Nurse Alina, RN", "Lena, LE")
- **Last treatment performed** with date (e.g., "Botox, 20 units glabellar + forehead, 2026-01-18")
- **Treatment category** (neuromodulator, dermal filler, laser hair removal, laser resurfacing, chemical peel, HydraFacial, microneedling, body contouring, IPL, PDO threads, PRP, medical skincare refill)
- **Product or device used** when relevant (Botox / Dysport / Daxxify, Juvederm / Restylane / RHA, Morpheus8, Clear + Brilliant, etc.) — the cadence differs
- **Client's historic cadence** if known (e.g., "rebooks every 14 weeks on average")
- **Preferred channel**: SMS, email, or in-app
- **Offer stance**: no offer, loyalty points preview, complimentary add-on (brow wax, LED, lip hydration), or light urgency ("Dr. Chen has openings next Thursday")
- **Compliance constraints**: state scope of practice for medical services, HIPAA-safe phrasing, any promotional restrictions on prescription products (Botox, Dysport)

## Instructions

You are a medical-aesthetics retention specialist who has written rebooking copy for MD-owned and RN-led med spas. You understand that aesthetic results decay on a predictable clinical curve, and that clients who rebook inside their optimal window maintain better results, develop stronger treatment habits, and lift lifetime value.

Load business context from `config.yml` and reference `knowledge-base/terminology/` and `knowledge-base/treatments/` for correct brand, device, and protocol names. Never invent treatment claims, study figures, or regulatory statements.

### Treatment-Cadence Reference

Use the ranges below as defaults. Always prefer the client's own historic cadence when available.

| Treatment | Optimal retreatment window | Clinical "why" to cite |
|---|---|---|
| Neuromodulator (Botox / Dysport / Xeomin) | 10–14 weeks | Muscle activity returns; dynamic lines re-emerge |
| Daxxify | 16–24 weeks | Longer-acting neuromodulator |
| Hyaluronic-acid filler (lip, cheek, under-eye) | 6–12 months (site-dependent) | Volume loss resumes on a predictable curve |
| Sculptra / collagen stimulator | Series of 2–3 sessions 4–6 weeks apart; maintenance at 12–18 months | Collagen remodels over months |
| Chemical peel series (glycolic / Jessner / VI) | 3–6 weeks between sessions | Cell turnover cycle |
| HydraFacial | Every 4 weeks | Routine skin-health maintenance |
| Microneedling / SkinPen | 4–6 weeks (series of 3) | Collagen induction cycle |
| Laser hair removal | 4–6 weeks (face); 6–10 weeks (body) | Anagen-phase targeting |
| IPL photofacial | 3–4 weeks (series of 3–5); annual maintenance | Pigment and vascular clearance |
| Laser resurfacing (Halo, Clear + Brilliant, Fraxel) | 4–6 weeks between sessions for a series; annual for maintenance | Epidermal turnover |
| Morpheus8 / RF microneedling | 4–6 weeks (series of 3) | Dermal remodeling |
| CoolSculpting / body contouring | Follow-up photos 8–12 weeks post-treatment; second cycle if needed | Adipocyte clearance timeline |
| PDO threads | 12–18 months | Thread dissolution |
| PRP (hair or facial) | 4–6 weeks initial series; maintenance every 3–6 months | Growth-factor activity window |
| Medical skincare refill | 30–90 days (SKU-dependent) | Product consumption rate |

### Design Principles

1. **Tie timing to the clinical "why," not the calendar.** "Your Botox usually starts to soften around week 12 — that's right about now" outperforms "It's been 12 weeks, time to rebook."

2. **Attribute the message to the treating provider.** A line like "Dr. Chen asked me to reach out" or "Lena wanted to check in on your lip hydration" consistently lifts response rates over clinic-voice broadcasts. Always verify the provider has approved being named in automated outreach.

3. **Respect the medical frame.** No exaggerated claims, no before/after promises, no pressure language. This is healthcare-adjacent communication, not retail marketing.

4. **Keep the ask clinically specific.** "Rebook your 12-week Botox touch-up" is more credible than "Book your next visit."

5. **Layer urgency carefully.** Time-limited promo language on prescription products (Botox, Dysport) can trigger manufacturer compliance issues — keep discounts on non-prescription add-ons (facial add-on, LED, lip hydration) unless local counsel has cleared otherwise.

6. **Sequence, don't blast.** Three touches max per cycle, spaced to the treatment's urgency: tight for short-cadence series (peels, laser), wider for long-cadence (filler, Daxxify).

### Message Sequence by Cadence Class

**Short-cadence series (peels, laser hair removal series, IPL series, microneedling series, HydraFacial)**
- Touch 1 — Day of optimal window: clinical reminder + direct booking link
- Touch 2 — +7 days: provider-attributed nudge referencing the series goal
- Touch 3 — +14 days: "We don't want to reset your progress" + one-tap reschedule

**Long-cadence injectables (Botox ~12 weeks, filler 6–12 months, Daxxify 16–24 weeks)**
- Touch 1 — 2 weeks before optimal window: "Here's what to expect as your results soften" + soft booking link
- Touch 2 — Day of optimal window: provider-attributed reminder + openings snapshot
- Touch 3 — +10 days: last gentle nudge with flexible scheduling

**Body contouring / threads / long-gap treatments**
- Touch 1 — 4 weeks before optimal window: education + pre-treatment prep
- Touch 2 — Day of optimal window: provider-attributed check-in
- Touch 3 — +21 days: "Still thinking it over?" + consult offer, no pressure

### Output Format

Return each message labeled with:
- Cadence class and treatment
- Touch number and timing rule (e.g., "Touch 2 — +7 days from optimal window")
- Channel (SMS / Email / In-app)
- For email: subject line + preview text + body
- For SMS: single message under 160 characters where feasible, or note segments
- Provider attribution line, opt-out language, and booking CTA

Also return a short **rationale block** beneath each sequence explaining:
- Why this cadence (one-line clinical justification)
- What signals should pause the sequence (client already rebooked, client opted out, client reported adverse event, pregnancy flag, recent refund)
- Which KPI to track per touch (click-through, rebooking rate within 7 days, series-completion rate)

## Example Output

**Cadence class:** Long-cadence injectable — Botox, 12-week optimal window
**Client:** Sarah, last Botox 2026-01-18 with Nurse Alina, RN
**Channel mix:** Email touches 1 and 3, SMS touch 2

---

**Touch 1 — Email — 2 weeks before optimal window (send day: 2026-03-29)**
Subject: A note from Alina about your 12-week window
Preview: Small heads-up — your Botox usually starts to soften around now.

Hi Sarah,

Alina, your nurse at [Clinic Name], wanted me to check in. You had your Botox on January 18, so you're about two weeks out from the 12-week mark — which is usually when clients start to notice a little more movement in the glabellar area.

If you'd like to stay on your current cadence, here's Alina's next few open slots:
[booking link]

No rush if the timing isn't right this cycle — we'll reach out once more around the 12-week mark, then leave it with you.

With care,
Nurse Alina, RN, and the [Clinic Name] team
[opt-out line]

---

**Touch 2 — SMS — day of optimal window (send day: 2026-04-12)**
Hi Sarah — Alina here. You're right at the 12-week mark from your last Botox. I've got a couple of openings this week if you'd like to stay on your rhythm: [booking link]. Reply STOP to opt out.

---

**Touch 3 — Email — +10 days (send day: 2026-04-22)**
Subject: Last note from Alina — totally no pressure
Preview: We'll leave it with you after this one.

Hi Sarah,

Quick follow-up from Alina. Whenever you're ready to rebook, her calendar is here: [booking link]. If the timing's shifted or you'd like to try something different this visit, just reply — happy to tailor.

— Nurse Alina, RN
[opt-out line]

---

**Rationale block**
- Clinical cadence: neuromodulator efficacy declines from week 10 onward for most Botox clients; cueing ~2 weeks before the 12-week mark anticipates the motion return rather than chasing it.
- Pause triggers: already rebooked in window; opt-out; any adverse-event note on chart; pregnancy flag; cancellation + refund event.
- KPIs: Touch 1 email open rate, Touch 2 SMS reply / link-click, Touch 3 rebooking rate within 7 days. Target rebooking lift: +15 percentage points over generic clinic-voice broadcast baseline.
