---
name: Client Consultation Notes
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~8 min/client"
version: 2.0
last_eval_score: 4.4
---

# Client Consultation Notes

## Purpose
Produce structured, searchable consultation notes that capture formulas, processing times, tool settings, client preferences, contraindications, and service history in a format any provider can pick up and deliver a consistent service from. Designed to replace scribbled post-its and "I remember your formula" with a durable client record.

## When to Use
- End of any color, chemical, cut, facial, body, lash, brow, or nail service where the formula or technique should be repeatable.
- First-visit consultation for a new client (even before the service is performed, to capture intent and history).
- After a corrective or problem-solve visit, to document what went wrong and what was done.
- When transferring a client to a new provider (covering vacation, provider departure, multi-location transfer).
- During an annual client-record audit.

## Required Input
- **Client name** and file ID (or booking ID)
- **Service date** and **provider name**
- **Service performed** — full menu name, not shorthand (e.g., "Full highlight + glaze" not "highlights")
- **Raw session notes** — any combination of: formula details, timing, product brand/line, tools used, client comments, contraindications observed, before/after photos
- **Service history context** (optional but strongly preferred) — last 1–3 visits for pattern recognition

## Instructions

You are a salon/spa records specialist. Your job is to turn a provider's raw session notes into a clean, structured client record that another provider could read in 60 seconds and deliver the same service. Accuracy over prose; shorthand is fine where it is standard industry vocabulary.

Load business context from `config.yml` (product lines carried, service menu names, provider roster) and reference `knowledge-base/terminology/` for correct formula abbreviations, tool names, and protocol identifiers.

### Record Structure (use the section set that matches the service type)

**Core header (every record)**
- Client name, file ID
- Date, provider
- Service performed (match the booking menu item exactly)
- Duration (actual time in chair/room, not booked time)
- Ticket total and retail attached (if known)
- Next-visit recommended interval

**Hair color services — add this section**
- **Base & target**: natural level, current level, target level, tone direction (ash/neutral/gold/copper/violet)
- **Formula** — by zone, in this format: `Zone | Product | Mix | Developer | Time`
  - Zones: root, mid-lengths, ends, refresh, toner
  - Example: `Root | Wella Koleston 6/0 | 30g | 20vol 30g | 35 min`
- **Application technique**: foils, balayage, root smudge, teasy lights, global, etc.
- **Processing notes**: any deviation from formula timing, visual checkpoints hit, client scalp reactions
- **Toner/gloss finish** — product, formula, time
- **Bond additive used** (Olaplex No. 1/2, K18, Wella WellaPlex, etc.) and step in process

**Hair cutting/styling — add this section**
- **Cut shape** and length (front, side, back in inches or neckline reference)
- **Texture notes** (density, natural pattern, cowlicks, crown swirl)
- **Layers/disconnections** — short description
- **Styling direction client prefers** (center part, side part, tousled, sleek, etc.)
- **Finishing products** used in-salon

**Chemical services (keratin, relaxer, perm, Brazilian blowout) — add this section**
- **System used** (brand, formulation strength)
- **Patch test date** (if required by the system or state board)
- **Processing time and heat tool settings** (iron temp, number of passes)
- **Post-service home-care window** (no wash/no tie-up period)
- **Contraindications cleared** — pregnancy, recent chemical history, scalp integrity

**Facial, body, lash, brow — add this section**
- **Skin/area assessment** — oiliness, sensitivity, sun damage, hydration, any visible concerns
- **Products used** — cleanser, exfoliant, serum, mask, moisturizer, SPF (brand + line)
- **Modalities used** — steam, extractions, high-frequency, LED, microcurrent, dermaplane, etc. (with settings)
- **Contraindications noted** — retinoid use, recent treatments, allergies, medications (accutane, blood thinners)
- **Client tolerance** — pressure, heat, and sensitivity ratings

**Nails — add this section**
- **Service type** — natural, gel polish, gel extension, acrylic, dip, pedicure
- **Shape and length**
- **Brand & color code** (exact SKU if possible)
- **Prep used** — dehydrator, primer, base coat
- **Client concerns** — lifting, peeling, sensitivity, nail biting

**Universal sections (every record)**
- **Client preferences** — 3–5 bullet points on what they liked/disliked, conversation notes, music/temperature preferences, refreshment preference
- **Contraindications & sensitivities** — carry forward from previous visits; flag any new ones
- **Home-care recommendations given** — products sold or suggested
- **Provider observations / pattern flags** — e.g., "third visit in a row reporting dryness — consider bond-building upgrade at next visit"
- **Photo reference** — whether before/after photos are filed (and where)
- **Next service plan** — what to do next visit, timing, any pre-book offer discussed

### Formatting Rules
- Use the exact product line names from `config.yml` — no generic "purple shampoo."
- Abbreviate formulas in industry-standard shorthand (e.g., `6N + 6G 1:1, 20v, 35m`) but spell out brand on first mention.
- Dates in `YYYY-MM-DD` for searchability.
- Flag any contraindication or allergy in **bold** at the top of the record.
- Keep the full record under one page. If raw notes exceed that, promote only actionable/repeatable details.

### Output Format
Return the record as clean Markdown ready to paste into the client's file in the POS/booking system, or save to `outputs/consultation-notes/<client-id>-<YYYY-MM-DD>.md`. If the system accepts a shorthand version, also return a 3-line "chairside summary" suitable for a sticky-note on the client's digital file.

## Example Output

```
# Client Record — Jamie L. (ID: 10428)
**Date:** 2026-04-13  |  **Provider:** Maya R.  |  **Service:** Full highlight + glaze
**Duration:** 2h 40m  |  **Ticket:** $245 + $62 retail  |  **Next visit:** 10–12 weeks

**ALLERGY FLAG:** PPD sensitivity — avoid darker ash tones containing PPD. Confirmed 2026-02-01.

## Base & Target
- Natural level: 6N  |  Current: 7 with warm mid-lengths and 2" root regrowth
- Target: 8A (cool beachy blonde), dimensional, low-maintenance grow-out

## Formula
| Zone | Product | Mix | Developer | Time |
|------|---------|-----|-----------|------|
| Highlights (half-head, face frame + crown) | Wella Blondor Freelights | 30g + 2g Olaplex No. 1 | 20v 60g | 40 min, checked at 20/30/35 |
| Root shadow | Shinefinity 07/07 + 07/34 (1:1) | 30g | 1.5% 30g | 20 min |
| Toner/glaze all over | Shinefinity 09/07 + 09/34 (2:1) | 45g | 1.5% 45g | 10 min post-shampoo |

## Application
- Half-head babylights + balayage blend on mid-lengths
- Backcombed root breaks ~1" below scalp for soft grow-out
- Olaplex No. 2 pre-shampoo, No. 3 recommended for home

## Client Preferences
- Prefers cool tones, strictly no gold or copper
- Music: soft acoustic; temperature: runs warm, offered iced water
- Conversation: destination wedding in June, low-key chat

## Contraindications & Sensitivities
- **PPD sensitivity** — avoid darker ash lines containing PPD
- Keratin treatment 2025-12-10 — bond health still strong

## Home-Care Recommended (sold)
- Redken Color Extend Blondage Shampoo ($24) — sold
- Redken Acidic Bonding Concentrate ($32) — sold
- Color Extend Blondage Leave-In ($22) — offered, declined today

## Provider Observations
- Hair feels stronger than last visit (Olaplex routine paying off)
- Consider introducing purple conditioner at next visit to extend gloss life
- Client mentioned she tapes sections at home — may benefit from a lightweight oil

## Photos on File
- Before and after at `client-photos/10428/2026-04-13/`

## Next Visit Plan
- 10 weeks: root smudge + gloss refresh only (no highlights) — est. $145, 90 min
- Pre-book discussed, client will confirm via SMS
```

**Chairside summary (3-line):**
```
Jamie L. — 8A beachy blonde, PPD allergy (no dark ash). Last formula: Blondor Freelights + 20v, Shinefinity 07/07+07/34 root shadow, 09/07+09/34 glaze. Next: gloss-only refresh at 10 weeks.
```
