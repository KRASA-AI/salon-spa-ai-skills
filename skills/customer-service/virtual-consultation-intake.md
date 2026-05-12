---
name: Virtual Consultation Intake Builder
category: customer-service
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~10 min/client"
version: 1.1
last_eval_score: 8.9
---

# Virtual Consultation Intake Builder

## Purpose
Generate a structured pre-visit digital consultation questionnaire and produce a stylist/therapist prep summary from the client's responses. Ensures the service provider walks into the appointment already understanding the client's goals, concerns, history, and expectations — reducing chair time spent on discovery.

The questionnaire and prep brief are pre-tailored to the practice's actual service menu, provider roster, and cadence class — not generic templates.

## When to Use
- New client onboarding (first-time visitors).
- Clients booking a significant change (new color, major cut, first-time facial or body treatment).
- Pre-consultation for bridal or event services.
- Virtual consultations where the provider reviews responses before a video call or in-person visit.
- When a client sends reference photos and you need to assess feasibility.
- Med-spa or clinical services (laser, neuromodulators, fillers, body contouring) where contraindication screening is required before the appointment.

## Required Input
- **Service category**: Hair color, haircut/style, facial, massage, body treatment, nail art, bridal, med-spa (laser / neuromodulator / filler / body contouring), or other
- **Client type**: New or returning
- **Consultation mode**: Pre-visit form (sent before appointment), live virtual (video call), or in-salon digital (tablet at check-in)
- **Any reference photos or inspiration shared by the client** (describe them if available)

## Instructions

You are a client experience coordinator for a salon or spa. Your job is to create a thorough but approachable intake questionnaire, then — once responses come in — synthesize them into a concise prep brief for the service provider.

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service and product names.

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Questionnaire greeting and prep brief header | "the salon" |
| `business.business_type` | Determines default question branch (salon / day spa / med spa) | infer from service category |
| `services.menu` | Pre-populates Section B service-specific questions with actual service names instead of generic placeholders | generic question text |
| `services.specialties` | Adds specialty-aware follow-ups (e.g., if balayage is a listed specialty, Section B asks about balayage-specific history) | omit specialty follow-ups |
| `services.cadence_class` | Pre-fills Section C maintenance frequency suggestion with the correct cadence window for the booked service | generic "how often would you like to come in?" |
| `staff.roster` | Provider Assignment Resolver — matches the booked provider by specialty and pre-fills their name in the prep brief header | "your stylist/therapist" |
| `retail.lines` | Adds brand-matched retail recommendation cue in the prep brief's Recommended Approach | generic "retail upsell" |
| `compliance.scope_of_practice` | Med-Spa Clinical Gate — if the service requires RN/NP/PA scope, adds required clinical contraindication questions to Section A | flag for user; do not skip clinical questions |
| `compliance.regulated_language` | Adds required disclosure to the Section D header for med-spa services | omit for non-clinical |

If a required config key is missing, generate the closest reasonable fallback and note it in the "Notes for form builder" at the bottom of the output.

### Service-Specific Section Generator

Before drafting Section B, identify the service's `cadence_class` from `config.yml.services.cadence_class`. Use the cadence class to determine:
1. **Which service-specific sub-questions to add** (see table below)
2. **What maintenance frequency to suggest in Section C** (pre-fill the options with the correct cadence window rather than a generic list)
3. **Whether the Med-Spa Clinical Branch applies** (any `med-spa-*` cadence class triggers the additional clinical intake in Section A)

| Cadence class | Section B service-specific questions | Section C maintenance suggestion |
|---|---|---|
| `color-cadence` | Current base color, prior chemical history (relaxer / keratin / perm), natural root %, inspiration photo | Every 4–6 weeks (gloss) / 8–10 weeks (full color) / 12–16 weeks (balayage grow-out) |
| `lash-lift` | Natural lash length and curl, prior lift history, eye sensitivity | Every 6–8 weeks |
| `brow-tint` | Natural brow color, prior tint history, skin sensitivity to oxidative color | Every 4–6 weeks |
| `gel-manicure` | Natural nail condition (thin, brittle, layered product history), any fungal history | Every 2–3 weeks |
| `nail-extension` | Extension type preference (hard gel / acrylic / soft gel), current length and shape reference | Every 3–4 weeks |
| `facial-cadence` | Current skincare routine (list products), areas of concern, prior professional treatments, known skin sensitivities, Fitzpatrick type if known | Every 4–6 weeks |
| `body-cadence` | Target area, prior body treatment history, pressure preference (massage), health conditions affecting treatment | Every 2–4 weeks |
| `med-spa-laser` | Skin type (Fitzpatrick I–VI), active tan, isotretinoin in past 6 months, history of keloid scarring, prior laser history on this area | Per protocol (4–6 weeks between sessions for most modalities) |
| `med-spa-neuromod` | Target areas, prior neuromodulator history (brand, units, injector, date), known resistance, bruising tendency | Every 3–4 months |
| `med-spa-filler` | Target areas, prior filler history (product, date, injector), autoimmune conditions, blood thinners, prior complications | Every 6–18 months (product-dependent) |
| `haircut-style` | Face shape, current length, styling tools used, inspirational cut references | Every 4–8 weeks |
| `bridal` | Wedding date, trial appointment date, bridal party size, any attending family receiving services | Trial 4–6 weeks prior; day-of appointment |

If `cadence_class` is not present in config and cannot be inferred, default to a generic four-section structure and flag the missing cadence class for the user to add to config.

### Med-Spa Clinical Branch

For any service with a `med-spa-*` cadence class, Section A must be expanded with clinical contraindication screening before the aesthetic questions. This is not optional — the expanded Section A feeds the provider's pre-treatment review and flags clients who need a pre-consult call before the appointment is confirmed.

**Expanded Section A (med-spa services only)**

*In addition to the standard About You questions, include:*

- Current medications (prescription and OTC, especially blood thinners, antibiotics, isotretinoin, immunosuppressants)
- Known allergies to injectables, topical anesthetics, metals (for laser targets), or hyaluronic acid products
- Active skin conditions in the treatment area (cold sore history for any laser near the face, active acne, eczema, psoriasis)
- Autoimmune conditions or immunocompromised status
- Pregnancy or breastfeeding status
- Prior adverse events or reactions to any aesthetic treatment
- For laser only: recent sun exposure or self-tanner use (within 4 weeks), isotretinoin use within 6 months, history of keloid or hypertrophic scarring

Flag the completed clinical section at the top of the provider prep brief as **"CLINICAL INTAKE — REVIEW BEFORE CONFIRMING APPOINTMENT."** If any red-flag response is present (blood thinner use, isotretinoin, active cold sore history, prior adverse event), add a **"Pre-Appointment Call Required"** banner at the top of the prep brief.

Reference `compliance.scope_of_practice` from config.yml for any service that requires provider credential verification before proceeding.

### Provider Assignment Resolver

Pull the assigned provider's name and specialty from `config.yml.staff.roster`. If no provider is pre-assigned:
- Match the service's `cadence_class` to the specialty column in the roster.
- Select the provider with that specialty (if multiple, list all and note "confirm assignment at booking").
- Pre-fill the prep brief header with the provider's name and their listed specialty / tenure note.

If `staff.roster` is empty or absent, use "your stylist/therapist/provider" throughout and flag the missing roster for the config.

### Cadence-Class Rebooking Hook

In Part 2's Recommended Approach, always include a **Rebooking Cue** sourced from the service's `cadence_class` maintenance window:

> *Rebooking cue: [Service name] typically rebooking in [cadence window]. Mention at checkout: "We typically see [service] clients back in [window] — want me to hold a spot before you head out?"*

This ensures the prep brief does double duty: provider prep *and* a prompted rebooking conversation starter. Route the rebooking follow-up to `customer-service/treatment-cadence-rebooking` if the client needs a full cadence explanation.

### Part 1: Generate the Intake Questionnaire

Create a friendly, conversational questionnaire appropriate to the service category. Keep it to 8–12 questions for standard services; up to 14 for med-spa clinical services (the expanded Section A adds questions). Group by section:

**Section A — About You (2-3 questions; 5-8 for med-spa)**
- Hair/skin type and current condition
- Any allergies, sensitivities, or medical considerations (e.g., pregnancy, medications affecting skin/hair)
- For returning clients: "Anything different since your last visit?"
- *For med-spa services: add full Med-Spa Clinical Branch questions above*

**Section B — Your Goals (3-4 questions)**
- Pre-fill with service-specific questions from the Service-Specific Section Generator above using `config.yml.services.menu` and `cadence_class`. Do not use generic placeholder text if config is present.
- What are you hoping to achieve today?
- Any inspiration images or references? (allow upload)
- Is there anything you definitely do NOT want?

**Section C — Lifestyle & Maintenance (2-3 questions)**
- How much time do you spend on daily styling/skincare?
- How often do you want to come in for maintenance? *(pre-fill options from `cadence_class` maintenance window — do not use a generic frequency list)*
- Budget comfort level for services and retail products (frame gently: "To recommend the right options, it helps to know your preferred investment range.")

**Section D — Anything Else (1 question)**
- Open text: "Is there anything else you'd like your [stylist/therapist/injector] to know before your appointment?"
- *For med-spa services: add `compliance.regulated_language` disclosure above the open-text field if present in config.*

### Part 2: Generate the Provider Prep Brief

When the client's completed responses are provided, produce a structured brief:

**Client Prep Brief — [Client Name] | [Provider Name from staff.roster]**
- **Service Booked:** [service from config.yml.services.menu — use the exact service name as listed]
- **Clinical Flag:** [CLINICAL INTAKE — REVIEW BEFORE CONFIRMING APPOINTMENT | Pre-Appointment Call Required] *(med-spa services only; omit for non-clinical)*
- **Key Goals:** 2–3 bullet points summarizing what the client wants
- **Important Notes:** Allergies, sensitivities, contraindications — for med-spa services, reproduce the clinical intake answers verbatim
- **Hair/Skin Assessment:** Based on client description (type, condition, history)
- **Reference Analysis:** If images were shared, note what's realistic given client's current state and suggest how to communicate adjustments
- **Recommended Approach:** A brief suggested plan for the provider, including products to prep from `config.yml.retail.lines` (use actual product names, not generic "color-safe shampoo"), estimated timing, and upsell opportunities
- **Rebooking Cue:** Cadence-class-sourced rebooking window and suggested chairside prompt (see Cadence-Class Rebooking Hook above)
- **Conversation Starters:** 1–2 talking points to build rapport based on the client's responses

### Output Format
Clearly label Part 1 (Questionnaire) and Part 2 (Prep Brief). The questionnaire should be ready to copy into a digital form tool (Typeform, Google Forms, salon software intake). The prep brief should be printable on a single page. Add a **"Notes for form builder"** section at the end noting any config keys that were missing or fell back to defaults.

## When Another Skill Owns This Job

| Situation | Owning skill |
|---|---|
| Client responses reveal a complaint or dissatisfaction before the visit | `customer-service/service-recovery-writer` (pre-empt with an outreach call) |
| Client is overdue for rebooking and this intake is their first touchpoint in 60+ days | `customer-service/client-winback-sequence` (run first; the intake is the warm-up) |
| Provider needs a full cadence explanation to give the client at checkout | `customer-service/treatment-cadence-rebooking` |
| Med-spa intake reveals a potential adverse-event concern | `operations/ai-consent-and-compliance-guardrails` (Review Checklist; do not confirm appointment before review) |
| Intake responses are used as the basis for a retail recommendation | `sales/retail-product-recommender` (pass Recommended Approach section as input) |
| Provider consultation notes need to be saved in the client record | `operations/client-consultation-notes` (the prep brief is the draft input) |

## Example Output

### Part 1 — Intake Questionnaire (Hair Color Service — balayage specialty, color-cadence)

*Config used: `services.specialties` includes balayage; `services.cadence_class` = color-cadence (balayage grow-out window: 12–16 weeks); `staff.roster` — Jen, colorist; `retail.lines` includes Oribe.*

**Welcome to [Salon Name]! We want to make sure your appointment is exactly what you're hoping for. Please take a few minutes to answer these questions before your visit with Jen.**

1. How would you describe your hair type? (straight, wavy, curly, coily)
2. Have you had any chemical treatments in the past 12 months? (color, relaxer, perm, keratin — select all that apply)
3. Do you have any scalp sensitivities or allergies to hair products?
4. What color result are you hoping for? Feel free to describe or upload a photo!
5. Is there anything you definitely want to avoid? (e.g., too warm, too dark, too much maintenance)
6. What is your current hair color — is it your natural shade or previously colored? If colored, when was your last service?
7. Have you had a balayage, highlights, or color melt before? If so, how did you feel about the result?
8. How often would you like to come in for color maintenance? (every 4–6 weeks for a toner refresh / every 12–16 weeks for a full balayage — which sounds like you?)
9. How much time do you spend on your hair each morning?
10. Are you open to product recommendations for maintaining your color at home?
11. Anything else you'd like Jen to know before your appointment?

---

### Part 2 — Provider Prep Brief

**Client Prep Brief — Jamie L. | Jen (Colorist)**

- **Service Booked:** Full balayage + gloss
- **Key Goals:** Brighter, sun-kissed blonde; less brassiness; low-maintenance grow-out in the 12–16 week window
- **Important Notes:** No known allergies. Previously had a keratin treatment 4 months ago.
- **Hair Assessment:** Medium-density wavy hair, currently dark blonde with warm undertones and visible root grow-out (~2 inches). Past highlights present but faded. Prior balayage — client was happy with the technique but wanted it lighter.
- **Reference Analysis:** Client shared two inspiration photos showing a cool-toned, beachy blonde. Achievable in one session given current base; recommend a toner in the ash-beige family to neutralize warmth.
- **Recommended Approach:** Foil-free balayage (face frame + scattered through mid-lengths and ends). Follow with a demi-permanent gloss (suggest 9NA + 9V mix). Estimated time: 2.5 hours. Retail prep: pull Oribe Bright Blonde Shampoo and Conditioner — client expressed openness to at-home color maintenance.
- **Rebooking Cue:** Balayage grow-out cadence is 12–16 weeks. Chairside prompt: *"We typically see balayage clients back in 12–16 weeks for a gloss refresh — want me to hold a spot before you head out?"*
- **Conversation Starters:** Jamie mentioned she's preparing for a destination wedding — ask about the event and suggest a style trial add-on. She's interested in low-maintenance home care — a 2-minute walkthrough of the Oribe Bright Blonde routine at the shampoo bowl lands well.

---

**Notes for form builder:** `services.specialties` and `staff.roster` used. `compliance.regulated_language` not present in config — no disclosure added (non-clinical service). If you add med-spa services in the future, populate `compliance.scope_of_practice` to trigger the clinical intake branch automatically.
