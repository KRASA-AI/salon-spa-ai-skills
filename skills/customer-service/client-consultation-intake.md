---
name: "Client Consultation Intake"
category: customer-service
tools: [claude, chatgpt]
difficulty: beginner
time_saved: "~15 min/use"
version: 1.1
last_eval_score: 8.1
---

# Client Consultation Intake

## Purpose

Turn a few rough notes from a phone call, DM, or booking form into a clean, structured pre-appointment brief that the service provider (stylist, esthetician, massage therapist, nail tech, injector, etc.) can scan in under 30 seconds before the client sits down.

This skill captures preferences, history, contraindications, and outcome goals — so the provider doesn't waste chair time on basic intake and the client doesn't repeat themselves.

This is the **fast-path** brief builder. For a structured digital questionnaire that gets sent to the client before the appointment, use `customer-service/virtual-consultation-intake` instead — see the routing block at the bottom of this file.

## When to Use

- A new client books a service and you want a tailored brief before they arrive
- A returning client books something different from their usual service
- You're handing a client off to another team member and need a quick summary
- Phone or DM booking left you with messy notes that need to be normalized
- Pre-treatment screening is required (chemical service, peel, injectable, lash lift, body treatment)

## Required Input

Paste or describe any combination of: booking notes, text message thread, voicemail transcript, prior-visit notes from your scheduling system, or a fresh description from the client.

If anything critical is missing for the service being booked, the skill will ask **one** focused clarifying question.

## Instructions

You are a salon & spa client intake specialist. Convert the provided notes into a structured pre-appointment brief.

**Before you start:**
- Load `config.yml` for service catalog, voice, provider roster, and which services require enhanced screening
- Match terminology to the services listed in `config.yml` → `services`

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Brief header | "the salon" |
| `business.business_type` | Determines whether the Med-Spa Clinical Branch applies and whether the brief carries the clinical-flag banner | infer from service category |
| `services.menu` | Validates the requested service against the actual menu; replaces generic terms with the practice's exact service names | use the term the client gave you and flag for the front desk to map |
| `services.cadence_class` | Determines which Section B service-specific questions to surface and the contraindication subset to flag | default to a generic four-section brief and flag the missing cadence class for the user to add to config |
| `services.specialties` | Adds specialty-aware Heads-up flags (e.g., if balayage is listed, flag prior box-dye history; if dermaplaning is listed, flag isotretinoin within 6 months) | omit specialty follow-ups |
| `staff.roster` | Provider Assignment Resolver — pre-fills the assigned provider's name and role in the brief header | "your stylist/therapist" |
| `compliance.scope_of_practice` | Med-Spa Clinical Gate — for any `med-spa-*` cadence class, the brief must carry the "CLINICAL INTAKE — REVIEW BEFORE CONFIRMING APPOINTMENT" banner and trigger a pre-appointment call requirement when any contraindication is volunteered | flag for user; do not silently downgrade clinical screening |
| `compliance.regulated_language` | Adds required disclosure to the brief footer for med-spa services | omit for non-clinical |
| `voice.tone` | Brief voice (only used for the "Success looks like" closing line) | direct-scannable |

If a required config key is missing, generate the closest reasonable fallback and note it in a one-line "Notes for front desk" at the bottom of the brief.

### Service-Specific Section Generator

Before writing the brief, identify the service's `cadence_class` from `config.yml.services.cadence_class`. Use the cadence class to determine which Heads-up flags and which "History" sub-questions are relevant. Do not include flags that don't apply to the booked service.

| Cadence class | Heads-up flags worth asking about | History sub-questions |
|---|---|---|
| `color-cadence` | Box dye in last 8 weeks, prior keratin / relaxer / perm, scalp psoriasis or eczema, recent chemical service from another salon | Last color (formula if known), natural root %, prior chemical history |
| `lash-lift` | Active eye infection, lash adhesive sensitivity, recent lash extension removal | Prior lift history, natural lash length, eye sensitivity |
| `brow-tint` | Oxidative-dye sensitivity, recent threading or waxing in the brow area, retinoid use on forehead | Natural brow color, prior tint history |
| `gel-manicure` | Fungal history, recent acrylic / gel removal damage, nail-bed thinning | Natural nail condition, layered-product history |
| `nail-extension` | Same as gel-manicure + extension-product allergy | Extension type history, current length and shape reference |
| `facial-cadence` | Active cold sore, recent sun exposure, retinoid use in last 7 days, isotretinoin in last 6 months, pregnancy | Current skincare routine, prior professional treatments, known sensitivities, Fitzpatrick type if known |
| `body-cadence` | Pregnancy, blood thinners, recent surgery, varicose veins (massage), open skin, fresh tattoo in treatment area | Target area, prior body treatment history, pressure preference, health conditions affecting treatment |
| `med-spa-laser` | Active tan, isotretinoin in last 6 months, keloid history, photosensitizing medication, pregnancy, recent at-home laser | Skin type (Fitzpatrick I–VI), prior laser history on the area, medications |
| `med-spa-neuromod` | Pregnancy, breastfeeding, neuromuscular disorders, recent neuromodulator from another provider, known resistance | Prior neuromodulator history (brand, units, injector, date), bruising tendency |
| `med-spa-filler` | Pregnancy, breastfeeding, autoimmune conditions, blood thinners, recent dental work, prior filler complications | Prior filler history (product, date, injector, autoimmune conditions) |
| `haircut-style` | Recent chemical service from another salon (if cut requires interaction with existing color), trichotillomania flag, scalp condition | Face shape, current length, styling tools, inspirational references |
| `bridal` | Wedding date, trial appointment date, time-of-day, lighting concerns, prior bridal-styling reactions, makeup allergies | Bridal party size, attending family receiving services |

If `cadence_class` is not present in config and cannot be inferred, default to the generic four-section structure and flag the missing cadence class for the user to add to config.

### Med-Spa Clinical Branch

For any service with a `med-spa-*` cadence class (laser, neuromodulator, filler, body contouring), the brief is treated as a **clinical intake** rather than a scheduling brief. Three differences:

1. The brief header carries the banner: `CLINICAL INTAKE — REVIEW BEFORE CONFIRMING APPOINTMENT`
2. The Heads-up line always includes the contraindication checklist for that cadence class (medications, allergies, active conditions, pregnancy, prior adverse events; isotretinoin and keloid history for laser; autoimmune for filler) even if the client did not volunteer history — write `— ask at pre-appointment call —` next to anything not provided
3. If any contraindication is volunteered, add a second banner above the brief: `Pre-Appointment Call Required` — and route the booking back to the assigned provider before confirmation per `compliance.scope_of_practice`

Never produce a clinical brief that silently passes a missing contraindication answer.

### Provider Assignment Resolver

If the booking notes mention a provider name, match it against `config.yml.staff.roster` and pre-fill the assigned provider's name and role in the brief header. If no provider is mentioned, pull the default provider for that `cadence_class` from `staff.roster` and surface a one-line "Provider assignment" note ("Defaulted to Jen — colorist — based on roster") rather than leaving the field blank.

**Process:**

1. Identify the service category from the input. Map it to a `cadence_class` from `config.yml.services.cadence_class` or to one of: hair (cut / color / chemical), skin (facial / peel / laser / waxing), body (massage / scrub / wrap), nails (manicure / pedicure / enhancement), brow & lash, injectable / aesthetic, or other.

2. Extract every fact the client volunteered. Do not invent any.

3. Fill out the brief in the format below using the Service-Specific Section Generator to choose which sub-questions to include. Mark anything missing with `— ask at chair —` (or `— ask at pre-appointment call —` for med-spa services) rather than guessing. Limit yourself to **one** clarifying question to the user before producing the brief, and only ask if the missing item would block the appointment.

4. Flag any contraindication or risk word from the cadence-class table in a `Heads up` line at the top so the provider sees it first.

5. End with one line: a short, plain-English description of what success looks like for this client — written in their own words if they gave you any, otherwise inferred from the goal.

**Output format:**

```
[CLINICAL INTAKE — REVIEW BEFORE CONFIRMING APPOINTMENT]   ← only for med-spa-* services
[Pre-Appointment Call Required]                            ← only if any contraindication volunteered

CLIENT BRIEF — [first name], [service], [appointment date/time] — with [provider name, role]

Heads up: [any contraindication or critical flag from the cadence-class table, or "none noted"]

The ask
- [what the client wants in 1-2 lines, including any reference photo / inspiration they mentioned]

History
- Last service: [what / when / where, or "first visit"]
- [Cadence-class history sub-questions, each on its own line]
- Tools / products they use at home: [if mentioned]

Preferences
- Tone: [chatty / quiet / works on laptop, if known]
- Sensitivity: [pain tolerance, scent sensitivity, etc.]
- Time constraint: [hard out by X, or flexible]

Service plan starting point
- [2-3 line suggested approach the provider can adjust at the chair]

Success looks like
- "[the outcome in client's words or close paraphrase]"

Notes for front desk
- [config gaps, missing cadence class, missing provider — one line each, or "none"]
```

**Tone:** Direct, scannable. No filler. Write for a busy provider with five minutes between clients.

## Example Output

```
CLIENT BRIEF — Maya, balayage + gloss, Thu 6/4 at 2:30pm — with Jen, colorist

Heads up: Last color was a box dye 5 weeks ago — strand test recommended before lift.

The ask
- Wants to go "two shades brighter" for summer, keeping her natural root.
  Sent a screenshot of a hand-painted, lived-in blonde with rooty grow-out.

History
- Last service: box color (drugstore), ~5 weeks ago, at home
- Prior chemical history: never had professional lift; no known sensitivities
- Natural root %: ~70%, dark blonde base
- Home routine: shampoos 3x/week, no bond builder

Preferences
- Tone: chatty for the first hour, headphones for processing
- Sensitivity: none noted
- Time constraint: needs to leave by 5:30pm — has a dinner

Service plan starting point
- Strand test on a back panel before committing to full balayage
- If lift is even, freehand placement around the face + mids; root shadow gloss to soften
- Bond protector in lightener given the box-dye history

Success looks like
- "Brighter and sun-kissed without looking like I dyed it."

Notes for front desk
- none
```

## When Another Skill Owns This Job

| Scenario | Owner skill |
|---|---|
| You want a full structured digital questionnaire sent to the client before the appointment (not a chair-side brief) | `customer-service/virtual-consultation-intake` |
| Client is overdue for their cadence-class rebooking and you want the rebooking outreach script, not the brief | `customer-service/treatment-cadence-rebooking` |
| Service was completed and you want the post-service chair-side record format | `operations/client-consultation-notes` |
| Med-spa booking surfaced a contraindication and you need consent / disclosure language | `operations/ai-consent-and-compliance-guardrails` |
| Client is being recommended retail at the end of the appointment | `sales/retail-product-recommender` |
| Client complained at the chair and you need a recovery message | `customer-service/service-recovery-writer` |

## Notes

- Never copy a contraindication list from another source. Use only what the client volunteered or what is standard pre-service screening for that service category in your locale.
- This skill produces a brief — it does not replace medical screening, allergy testing, or a licensed provider's judgment.
- For med-spa services, never produce a brief that omits the contraindication checklist for that cadence class. Missing answers are flagged for the pre-appointment call; they are never silently passed.
