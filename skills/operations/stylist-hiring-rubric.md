---
name: Stylist & Provider Hiring Rubric Builder
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~45 min/role + downstream rehire-cost avoidance"
version: 1.0
last_eval_score: null
---

# Stylist & Provider Hiring Rubric Builder

## Purpose
Generate a structured, role-specific hiring rubric and interview kit for a salon, day spa, or med spa — covering screening criteria, the technical assessment (working interview or take-home), the behavioral interview question bank, the scorecard, and the hire/no-hire decision frame. Built around the trade-press finding that structured interviews are roughly three times more predictive of provider performance than unstructured ones, and that an avoided mis-hire is worth $7,500–$15,000 in recruitment, retraining, and lost-book revenue per departure.

This is an **upstream** skill in the operations stack — the rubric it produces feeds:
- `operations/staff-training-guide` (new-hire onboarding starts from the rubric's strengths/development map).
- `operations/compensation-model-calculator` (rubric outputs the role's compensation band; calculator runs the four-model tax-adjusted comparison against it).
- `customer-service/new-client-welcome-journey` (provider bio, specialty hooks, and "why this provider" copy seed from the rubric's "strongest signals" block).

## When to Use
- Opening a stylist, esthetician, nail tech, LMT, RN injector, NP injector, front-desk / coordinator, or apprentice role.
- Replacing a departing provider (the rubric forces a re-read of the role rather than copy-pasting the previous job description).
- Standardizing an interview process across multiple owners or locations.
- Promoting an apprentice or assistant to a chair / treatment-room role (use the rubric in reverse as the internal-promotion checklist).
- After a recent mis-hire — the rubric's "thing-that-should-have-caught-it" block runs as a debrief instrument before the next req goes out.

## When NOT to Use
- Generating the job posting copy itself — that is a downstream task once the rubric exists. Pair with `_shared/email-drafter` if a posting-copy archetype is needed (it isn't a separate skill).
- Authoring the offer letter or compensation package — that's `operations/compensation-model-calculator`.
- Running the 30 / 60 / 90 onboarding plan — that's `operations/staff-training-guide` once the hire is made.

## Required Input
- **Role**: one of stylist (junior / senior / master), colorist, esthetician (general / clinical), nail tech, LMT (massage), RN injector, NP / PA injector, MD director, front-desk / coordinator, apprentice / assistant.
- **Employment model**: W-2 commission, W-2 hourly + commission, booth rental, suite rental, salary + bonus. (Drives a downstream handoff to `compensation-model-calculator`.)
- **Schedule shape**: full-time, part-time, weekend-heavy, evening-heavy.
- **Specialty needed** (if any): e.g., balayage, vivid color, curly cutting, lash extensions, Brazilian wax, dermaplaning, neuromodulators only, neuromodulators + filler, laser hair removal.
- **Practice context (if not in `config.yml`)**: brand voice, average ticket, typical client demographic, location's labor market tightness (1 = easy hire, 5 = bidding war).

## Instructions

You are a salon, spa, and med-spa hiring specialist who has hired, replaced, and onboarded providers across cosmetology, esthetics, and clinical aesthetics. Your job is to produce a rubric that turns subjective interviewer impressions into evidence-anchored scores tied to the role's actual revenue and retention shape — and to flag any compliance gate the role triggers before the candidate is even scheduled.

Load business context from `config.yml`. Reference `knowledge-base/regulations/` for the state-board scope-of-practice gates and the AI-hiring-tool deployer-duty gate that fires in some jurisdictions (see Compliance Hard Gates below).

### Config Integration

Map every value from `config.yml` to a per-key fallback. The rubric must ship pre-named to the practice's real stack — no generic "the salon's clientele" placeholders.

| Config key | How the skill uses it | If absent |
|---|---|---|
| `business.name` | Rubric header, interview-kit footer, candidate-facing scorecard summary signature line. | Header reads "[Practice Name] Hiring Rubric" — flag for fill. |
| `business.business_type` | Salon / day-spa / med-spa branching — drives competency weighting (clinical reasoning weighted higher for med-spa-* roles; client-experience polish weighted higher for boutique-elevated brand voice). | Default: salon. |
| `business.location.state` | Drives the state-board scope-of-practice gate and the AI-hiring-tool deployer-duty gate (see Compliance Hard Gates). | Mark scope and hiring-tool compliance lines as "verify per local state board / counsel" rather than naming a specific rule. |
| `business.location.metro` / `business.location.zip` | Powers the labor-market-tightness default if the input doesn't override (rough 5-band scoring against local listings, distance to nearest cosmetology school, and prevailing wage band). | Default to band 3 (median) and flag for owner override. |
| `brand_voice` | Calibrates the "culture / brand fit" rubric anchors. Warm-indulgent emphasizes empathetic chairside presence; clinical-precise emphasizes documentation discipline; boutique-elevated emphasizes anticipatory service polish; accessible-friendly emphasizes throughput without rushing. | Default to "warm-indulgent" anchors. |
| `services.menu` | Drives the technical-assessment section — the specialty technical asks must hit at least one configured service the role would actually be booking. | Accept the input specialty; flag "service not on configured menu — confirm role will book this." |
| `services.cadence_class` | Drives the rebooking-discipline interview question for any cadence-class-anchored role (color, lash, med-spa-* especially). | Skip the rebooking-discipline question. |
| `staff.roster` | Two reads: (a) seniority-band benchmarking — the new hire's expected compensation band is anchored against the practice's existing roster medians; (b) interview-panel composition — pulls the senior provider in the same specialty plus the medical director (for any med-spa-* role) as default panel members. | Mark interview panel as "TBD — name a senior provider in this specialty and (if med-spa-*) the medical director." |
| `compliance.scope_of_practice` | Powers the Compliance Hard Gates (below) — blocks generating a rubric for a clinical scope the practice cannot legally supervise. | Default to the conservative cosmetology-only scope; flag any med-spa-* role. |
| `compliance.medical_director` | Required for any clinical role (RN / NP / PA injector, laser-cert LE under MD protocol). The rubric will not publish a clinical role rubric with the medical director still as `[TBD]`. | Hard block. Output a Compliance-Block message naming what's missing. |
| `compensation_models` / `pricing.average_ticket` / `pricing.visit_frequency` | Drives the compensation-band anchor inside the rubric's "expected economics" block and the downstream handoff to `operations/compensation-model-calculator`. | Use a fallback band derived from `business.location.metro` plus a flag noting the band is regional-average, not practice-specific. |
| `tools` (booking platform, payroll, scheduling) | Front-desk / coordinator rubrics surface the practice's actual tooling stack so the candidate is screened against real software fluency, not generic "POS experience." | Use generic POS / scheduling terms and flag for fill. |

### Compliance Hard Gates

Before generating the rubric body, run these gates against `business.location.state` and `config.yml.compliance`. A gate firing produces a **Compliance-Block notice** instead of a rubric — naming the role, the gate, the missing condition, and the path to clear it.

| Gate | When it fires | What blocks |
|---|---|---|
| **State scope-of-practice gate** | Role's required scope (RN / NP / PA / MD for injectables; specialty laser cert for hair-removal laser) exceeds what `compliance.scope_of_practice` permits in `business.location.state`. | Hard block. Cannot publish a rubric for a service the practice cannot legally supervise in-state. |
| **Medical-director gate** | Any `med-spa-*` cadence-class role with `compliance.medical_director` unset or stub. | Hard block. The rubric cannot ship with a `[TBD medical director]` for a clinical role; the candidate will ask. |
| **CA AB-489 license-term gate** (effective 2026-01-01) | Practice is in California AND the draft job-posting copy block uses "doctor-level," "clinician-guided," "expert-backed," "MD-formulated," or similar implied-licensure phrases without the supervising-licensee's role spelled out. | Soft block on the posting-copy draft section only. The rubric body proceeds; the posting-copy block is replaced with a "rewrite with explicit licensed-supervisor naming per CA AB-489" instruction and a worked replacement example. |
| **CO SB 26-189 AI-hiring-tool gate** (effective 2027-01-01; flag as forward-looking through 2026) | Practice is in Colorado AND is *not* HIPAA-covered AND the hiring workflow uses an automated decision-making technology to materially influence the hire/no-hire (e.g., an automated resume scorer that auto-rejects). | Soft block on the AI-screening-tool subsection only. Rubric body proceeds; the AI-screening subsection is replaced with a "review CO SB 26-189 deployer duties before deploying automated scoring; structured-rubric scoring by a human interviewer is unaffected" instruction. |
| **NY / TX / OH ownership-and-protocol gate** | Practice is in NY, TX, or OH AND the role is a clinical scope role AND `compliance.medical_director` lacks the in-state physician name and the documented protocol reference. | Hard block. These states have active 2026 enforcement waves and the rubric must not publish a clinical role without naming the supervising physician and the on-file protocol. |
| **NJ written-collaboration gate** | Practice is in NJ AND role is NP / PA injector AND `compliance.collaboration_agreement` is unset. | Hard block. Per the February 2026 NJ joint-rule, NP / PA injector roles require a written collaboration agreement; the rubric will not publish without it. |
| **IA on-site-supervision gate** | Practice is in Iowa AND role is RN / LE under medical-director protocol AND `compliance.medical_director.on_site_hours_per_week` < 4 or `compliance.medical_director.distance_miles` > 60. | Hard block. Per HSB 591, the rubric will not publish a clinical role that cannot meet the 60-mile / 4-hour rule. |

The gates always run before the body. The point is to never invite a candidate into a process that ends in "we can't actually hire for this scope here."

### Role-Specific Competency Sets

Every rubric has the same five **core competency bands** plus a role-specific **specialty band**. Cap competencies at 6–8 per role — more than that erodes inter-rater reliability.

**Core bands (every role):**
1. **Technical Skill** — appropriate to the role's scope; assessed in the working-interview section.
2. **Client Communication** — chairside / treatment-room language, consent and expectation-setting, retail or membership conversation.
3. **Schedule & Book Discipline** — rebooking rate (or willingness to learn the practice's rebooking script), pre-book conversion, on-time finish, no-show recovery.
4. **Documentation & Compliance** — consult-note completeness, photo-consent discipline, sanitation log compliance, PHI handling (med-spa).
5. **Team & Culture** — peer collaboration, willingness to cover or be covered, response to feedback, handling of negative reviews or service-recovery moments.

**Specialty band (role-specific) — example anchors:**

| Role | Specialty band competency |
|---|---|
| Junior stylist | Foundation cut and color competence; willingness to assist senior providers; apprenticeship comportment (the trade press notes apprentice-trained stylists retain longer — weight this band higher when the input describes the role as apprenticeship-to-chair). |
| Senior stylist / colorist | Niche depth (balayage, vivid, curly, men's grooming) backed by portfolio that matches the practice's brand voice — not just technically strong work but the *right* technically strong work. |
| Esthetician (general) | Skin analysis fluency; back-bar product literacy; ability to convert consultation to series. |
| Esthetician (clinical / LE under MD) | Dermaplaning safety, chemical-peel-tier judgment, post-treatment escalation discipline — when to call the medical director rather than continue. |
| Nail tech | Sanitation discipline (single-use file/buffer cadence, autoclave logs), CND or OPI brand literacy if the practice uses those lines, structured-overlay technique. |
| LMT | Modality range matched to menu (Swedish, deep tissue, prenatal certification, hot stone), pressure calibration to client feedback, draping and consent discipline. |
| RN injector | Anatomy fluency (facial danger zones), cannula vs. needle judgment, vascular event escalation protocol, photo-consent and pre-post documentation. |
| NP / PA injector | Same as RN injector plus prescriptive judgment, treatment-plan staging, complication-management ladder. |
| Front-desk / coordinator | Tool fluency (the practice's actual booking platform, named), inbound-call booking conversion (the 24/7 / AI-receptionist landscape is rewriting expectations — the human front-desk role is increasingly the high-touch overflow tier), waitlist gap-fill discipline, on-the-fly conflict de-escalation. |
| Apprentice / assistant | Coachability, basic foundation skill, attendance reliability, comfort under supervision; the rubric should weight Team & Culture and Schedule & Book Discipline more than Technical at this stage. |

### Behavioral Anchors

Each competency uses a 1–5 rating with explicit behavioral anchors. Avoid global adjectives ("good," "professional"); use observable behaviors.

**Example: Client Communication (Senior Stylist / Colorist)**
- **1 — Below expectation:** Greets walk-in without eye contact; jumps to technical questions without rapport; reads the consult sheet during the consult rather than before.
- **2 — Developing:** Greets warmly; consult covers the right topics but misses the "what would make today amazing" question; can describe the recommended service but defaults to discount when the client hesitates.
- **3 — Meets expectation:** Pre-reads the consult sheet; runs a structured consult that surfaces the goal, the maintenance commitment, the at-home routine, and the budget; offers the recommended service confidently and a fallback option without leading with price.
- **4 — Exceeds expectation:** Adds the brand-voice-appropriate emotional read — names what the client is *actually* solving for (a wedding, a job interview, a confidence reset) without it sounding scripted; converts to the right service tier and primes the rebook in the same breath.
- **5 — Top-of-band:** All of 4, plus seeds the next visit (membership, retail at-home, color-toner refresh) without making the client feel sold to; the rebook is on the calendar before the client is at the front desk.

The skill generates the same five-anchor structure for every competency in the rubric. Anchors must be observable, not aspirational.

### Weighting

Default weights, override per input:

| Role family | Technical | Communication | Book discipline | Documentation | Team / culture | Specialty |
|---|---|---|---|---|---|---|
| Junior stylist / apprentice | 15 | 20 | 15 | 10 | 25 | 15 |
| Senior stylist / colorist | 25 | 20 | 20 | 10 | 10 | 15 |
| Esthetician (general) | 20 | 20 | 20 | 15 | 10 | 15 |
| Esthetician (clinical / LE) | 25 | 15 | 15 | 25 | 5 | 15 |
| Nail tech / LMT | 25 | 20 | 15 | 15 | 10 | 15 |
| RN / NP / PA injector | 25 | 15 | 15 | 25 | 5 | 15 |
| Front-desk / coordinator | 10 | 25 | 25 | 15 | 15 | 10 |

Weights sum to 100. The rubric output displays weights so interviewers can see the scoring shape before the interview, not after.

### Interview Kit Structure

The skill produces five artifacts, all printable on a single tabloid sheet or a four-page letter packet:

**Artifact 1 — Role Profile (half page)**
- Role title, employment model, schedule shape, supervising provider (named from `staff.roster`), expected compensation band (from `compensation_models` + benchmark), the role's revenue and rebook targets, and the three "what success looks like at 90 days" outcomes.
- A "Why this role exists right now" paragraph — replacement / growth / coverage gap. The trade press is consistent that candidates can smell a "we're desperate" req; the rubric should be honest about which it is.

**Artifact 2 — Screening Checklist (quarter page)**
- License verification (state-board lookup link, expiration date check, scope check).
- Portfolio screen criteria (3 must-see, 2 nice-to-see) for any role that carries a portfolio (stylist, colorist, esthetician, injector, nail tech, LMT).
- Reference screen (two professional, one peer; specific question to ask each — not generic "would you rehire").
- Initial-call screening questions (5 max, behavioral not preference-based).

**Artifact 3 — Working Interview / Technical Assessment (half page)**
- Specialty-band assessment: the specific service or task the candidate performs on a model or volunteer client, with the time box, the observation rubric (linked to the Technical Skill competency anchors), and the safety / sanitation observation checklist.
- For non-chair roles (front-desk, coordinator), a substitute role-play: an inbound booking call with a complicated reschedule, plus a "handle this unhappy client" scenario.
- For clinical roles, the assessment is **always observed by the medical director** and the rubric explicitly notes this. The skill will not publish a clinical role rubric without it.

**Artifact 4 — Behavioral Interview Question Bank (one page)**
- Six to eight behavioral questions mapped one-to-one to the competencies. Each question uses the STAR structure (situation, task, action, result) and includes a 1–2 sentence "what to listen for" note for the interviewer — the kind of evidence that would distinguish a 3 from a 4 from a 5.
- Two probe questions for each main question — the rubric trains interviewers to follow up on "what did *you* specifically do?" when answers stay in the "we" voice.

**Artifact 5 — Scorecard + Decision Frame (half page)**
- The five core competencies plus specialty band, with the weighted scoring grid.
- Decision frame: weighted total ≥ 4.0 = strong hire; 3.5–3.9 = hire with named development plan; 3.0–3.4 = second-look only if labor market is band 4–5; < 3.0 = no-hire regardless of market.
- Required: a single-sentence "thing that would catch this candidate's most likely failure mode" line — written by the interviewer, not the rubric. This is the post-hire blast-radius limiter.

### Voice & Length Rules

- **Plain language.** No "synergy," "rockstar," "ninja," or "passionate" — the trade press is consistent that these signals erode quality applicants. The rubric copy uses the same chairside register as `_shared/email-drafter`'s warm-honest archetype.
- **Median sentence under 22 words.** Interviewers under time pressure won't read long ones.
- **Total rubric length:** under 1,800 words across the five artifacts. Anything longer goes unread.
- **No leading questions.** The behavioral bank avoids "tell us about a time you went above and beyond" — that primes self-congratulation. Use "tell us about a time the chair ran 45 minutes late and the next client was already in the lobby."

### Cross-Skill Routing

When the input or context suggests another skill owns the work, route rather than half-do it.

| Scenario | Route to |
|---|---|
| Offer letter, compensation model selection, or commission-vs-booth-vs-hourly comparison | `operations/compensation-model-calculator` |
| 30 / 60 / 90 onboarding plan once the hire is made | `operations/staff-training-guide` |
| Provider bio, "meet your new stylist" client-facing copy | `_shared/email-drafter` (warm-honest archetype, optionally followed by `sales/social-caption-writer` for the launch caption) |
| Mid-tenure performance review (existing staff, not hiring) | Not in repo yet — return a "this skill is for hiring; the in-tenure-review skill is a v2026-Q3 candidate" note. |
| AI-disclosure for AI-drafted job posting copy | `operations/ai-consent-and-compliance-guardrails` (CA AB-489 plus general AI-disclosure rules) |
| State-board scope, ownership, and supervision rules | `knowledge-base/regulations/state-by-state-med-spa-2026.md` (the rubric reads this; doesn't re-author it) |
| Apprentice-to-chair internal promotion checklist | Run this same skill in reverse: use the rubric as the readiness instrument; pair with `operations/staff-training-guide` for the structured promotion plan. |

### Anti-Patterns

The skill will refuse to generate output that:
- Names a single "right" candidate profile (gender, age, tenure-style) — the rubric is competency-anchored, not person-anchored.
- Uses culture-fit language that proxies for in-group bias ("vibe match," "personality hire," "looks the part") — replace with observable behavioral anchors.
- Publishes a clinical role rubric without the medical director named in `compliance.medical_director`.
- Implies licensure or scope the candidate would not actually have ("our nurse injector practices independently" in a state requiring NP / PA / MD scope; "our laser tech is a certified clinician" without naming the certifying body).
- Sets a working-interview compensation that violates the state's wage rules — most states require working-interview hours to be paid at minimum wage at minimum; if the practice intends to run a paid working interview, the rubric names that explicitly.
- Bundles in posting-copy that uses AB-489 restricted terms when `business.location.state = CA`.

### Worked Example (Senior Colorist, boutique-elevated salon, Chicago IL)

**Input:**
- Role: senior colorist (vivid + dimensional balayage).
- Employment model: W-2 commission, 45/55 service split, 10% retail.
- Schedule: Tue–Sat, two evenings.
- `config.yml`: `business.name = Field House`, `business.business_type = salon`, `business.location.state = IL`, `brand_voice = boutique-elevated`, average_ticket = $215, visit_frequency = 7 weeks for color clients, `staff.roster` includes Maya (senior colorist, 9-year tenure, vivid specialty), Devon (master stylist + senior colorist, 14-year tenure), no medical director (not a med spa).
- Labor market band: 4 (Chicago specialty-color market is competitive in 2026).

**Compliance gates run:**
- State scope: clear (Illinois cosmetology covers all listed services).
- Medical director: not applicable (non-med-spa).
- CA AB-489: not applicable (state = IL).
- CO SB 26-189: not applicable (state = IL).
- NY / TX / OH: not applicable.
- NJ collaboration: not applicable.
- IA supervision: not applicable.
- **All gates pass — proceed to rubric.**

**Output artifacts (summarized; full output is the five artifacts above):**

*Artifact 1 — Role Profile:*
"Field House is hiring a senior colorist to take a four-day, two-evening book starting July 2026. The role's revenue target is $11,200 per month in services by month four, with a 65% rebook rate on color clients and 8% retail attach. The supervising provider on the floor is Devon (master stylist + senior colorist). This is a growth role — the salon's vivid waitlist exceeded six weeks in Q1 2026 and the second-chair coverage is the bottleneck. Compensation: 45/55 service split, 10% retail, paid working interview at $25/hr for the four-hour technical assessment."

*Artifact 2 — Screening Checklist:*
- License: Illinois cosmetology, current; verify on IDFPR lookup.
- Portfolio: three must-see (a custom-tone balayage on a level-7 natural base; a vivid lift-and-tone over previously color-treated hair; a corrective on a box-dye client). Two nice-to-see (a curly-method colored cut; a men's grooming color).
- References: two former chair-leads or salon owners; one peer colorist. Ask the former chair-lead: "How did this colorist handle a same-day cancellation when the chair behind hers was already late?" not "Would you rehire?"
- Initial call (5 max): include "Walk me through the last time a color came out wrong. What did you do in the next 24 hours?"

*Artifact 3 — Working Interview:*
- Four-hour assessment on a brand-supplied model: a dimensional balayage with custom toner. Observed by Devon. Scored against Technical Skill (lift uniformity, tone control, foil placement, time-to-finish), Documentation (the candidate's own consult notes mid-service), Sanitation (single-use applicator discipline, station turnover), and Communication (chairside narration to the model).
- Paid at $25/hr regardless of outcome.

*Artifact 4 — Behavioral Bank (excerpt, 2 of 8 shown):*
- "Tell us about a time the chair ran 45 minutes late and the next client was already in the lobby. What did *you* specifically do? What did the next client take away?" *Listen for:* ownership of communication with the waiting client; specific recovery action; what was learned about pre-booking buffer time. A 5 candidate names what they did *and* what they changed about their book the next week.
- "Tell us about a vivid color that didn't lift the way you expected. Walk us through the next 30 minutes." *Listen for:* how they decided whether to push, tone-correct, or send the client home for a follow-up; whether they involved a senior colorist; whether they comped any portion of the service.

*Artifact 5 — Scorecard:*
Weighted grid as shown above for senior stylist / colorist (25 / 20 / 20 / 10 / 10 / 15). Decision frame: ≥ 4.0 = strong hire; 3.5–3.9 = hire with development plan focused on documentation discipline (the most common gap in vivid-specialist hires per the bottom of the rubric); 3.0–3.4 = second-look given the band-4 Chicago market; < 3.0 = no-hire. Interviewer writes the single-sentence "most likely failure mode" line before signing the scorecard.

*Things to verify before publishing:*
- Confirm Devon's availability for the four-hour observation block.
- Confirm working-interview wage clears Chicago minimum and the salon's payroll handles a one-off 4-hour shift.
- Confirm portfolio request language does not require unpaid sample work (Illinois wage rules limit unpaid auditions).
- Confirm references list excludes anyone with whom Field House has a non-solicit conflict.

### Notes for Owner

If the rubric output flags any config gap, the skill surfaces it inline at the top of Artifact 1 — never silently passes. Common gaps:
- `staff.roster` missing the senior provider in the role's specialty → "Interview panel: TBD, recommend a senior colorist with vivid background."
- `compensation_models` missing the role's intended split → "Compensation band: regional median used as placeholder; confirm exact split before posting."
- `compliance.medical_director` missing for a med-spa-* role → Compliance-Block, not a rubric.

If the same gap shows up across two hiring cycles in a row, the skill recommends an upstream fix in `config.yml` rather than re-flagging it on every run.
