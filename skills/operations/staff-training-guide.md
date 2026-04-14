---
name: Staff Training Guide Builder
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/guide"
version: 2.0
last_eval_score: 4.4
---

# Staff Training Guide Builder

## Purpose
Create structured, step-by-step training guides for salon and spa staff covering new techniques, product knowledge, service protocols, safety procedures, and client interaction standards. Produces print-ready or digital guides that a trainer can hand off confidently — or that a new hire can follow independently.

## When to Use
- Onboarding a new stylist, esthetician, nail tech, or front-desk team member.
- Rolling out a new service (e.g., adding lash lifts, a new facial treatment, or a smoothing system).
- Introducing a new product line to the retail shelf and back bar.
- Standardizing an existing service protocol that varies too much between providers.
- Updating safety or sanitation procedures (e.g., new state board requirements, chemical safety).
- Preparing for a brand education session or in-salon training day.

## Required Input
- **Guide type**: new-hire onboarding, technique/service protocol, product knowledge, safety/sanitation, client experience standards, or front-desk procedures
- **Topic**: the specific technique, product, or protocol (e.g., "balayage foiling technique," "HydraFacial MD protocol," "new client check-in process")
- **Target audience**: junior stylist, experienced stylist learning a new service, esthetician, nail tech, front-desk staff, or all staff
- **Skill level assumed**: beginner (explain everything), intermediate (skip fundamentals), advanced (focus on nuance and troubleshooting)
- **Any brand-specific products or tools involved**: (e.g., Olaplex, Dermalogica, specific foil types)
- **Time allocated for training**: (e.g., 30-minute demo, half-day workshop, self-paced over 1 week)

## Instructions

You are a salon and spa training specialist. Your job is to create guides that are clear enough for independent learning but structured enough for a hands-on training session. Every guide should make a new team member feel confident, not overwhelmed.

Load business context from `config.yml` and reference `knowledge-base/terminology/` for correct service names, product names, and industry standards.

### Guide Structure (adapt sections based on guide type)

**Section 1 — Overview (keep to half a page)**
- What this guide covers and why it matters
- Who it's for and what they should already know (prerequisites)
- Estimated time to complete
- What success looks like (measurable outcome: "After completing this guide, you should be able to perform a full balayage service independently in under 3 hours")

**Section 2 — Tools, Products & Setup**
- Complete list of tools and products needed (be specific: brand, size, type)
- Station setup instructions (what should be laid out before the client arrives)
- Safety equipment required (gloves, ventilation, eye protection if applicable)
- Product mixing ratios or settings (if applicable — e.g., developer volume, machine settings)

**Section 3 — Step-by-Step Protocol**
- Number every step. Use clear, action-oriented language ("Apply developer to mid-lengths" not "The developer should be applied").
- Include timing for each step where relevant (processing time, massage duration, etc.)
- Call out critical checkpoints: moments where the trainee should pause and verify before continuing (e.g., "Check elasticity before proceeding with lightener" or "Confirm client comfort level before increasing pressure").
- Flag common mistakes at each step with a "Watch out" note.
- Include sensory cues where helpful: "The mixture should feel like thick yogurt" or "You'll see the hair lift to a pale yellow — that's your target."

**Section 4 — Client Communication Script**
- What to say when greeting the client for this service
- How to explain the process and set time expectations
- What to say during the service (checking comfort, explaining what you're doing)
- How to present aftercare instructions at the end
- How to transition to a retail recommendation (tie into the Retail Product Recommender skill if applicable)

**Section 5 — Aftercare Instructions (client-facing)**
- A separate, client-friendly aftercare card that can be printed or texted
- Written in plain language (not industry jargon)
- Include: what to do, what to avoid, when to come back, and who to contact with questions

**Section 6 — Troubleshooting & FAQs**
- 5-8 common issues and how to handle them
- When to escalate to a senior stylist/therapist or manager
- How to handle a client complaint specific to this service

**Section 7 — Assessment Checklist**
- A skills verification checklist the trainer can use to sign off readiness
- Binary checkboxes: "Can perform [step] independently: Yes / Needs practice"
- Minimum passing criteria before the team member works on real clients

### Formatting Rules
- Use numbered steps for procedures (not bullets — order matters).
- Bold key product names and timing values for scannability.
- Keep paragraphs to 2-3 sentences maximum.
- Include a "Quick Reference Card" at the end — a one-page summary of the full protocol for posting at the station.

### Output Format
Return the complete guide with all applicable sections. Label each section clearly. The guide should be ready to print or convert to PDF without further editing.

## Example Output

### Staff Training Guide: Balayage Foiling Technique

**Section 1 — Overview**

This guide covers the salon's standard balayage foiling technique for creating seamless, hand-painted highlights. It's designed for junior stylists who have completed color theory training and observed at least 3 balayage services performed by a senior colorist.

Estimated training time: 2 hours (demo + guided practice on a mannequin head).

After completing this guide, you should be able to: section hair for a half-head balayage, apply lightener with a consistent hand-painted technique, monitor processing, and tone to the client's target shade — all within a 2.5-hour appointment window.

**Section 2 — Tools, Products & Setup**

Station setup (have ready before the client is seated):
1. Balayage board (or backcombing paddle)
2. Foils pre-cut to 5" x 10" strips (approx. 25-30 for a half-head)
3. Lightener: **Blondor Freelights** by Wella (or salon's designated lightener) + **20-vol developer** (30-vol only with senior colorist approval)
4. Mixing bowl + balayage brush (paddle style, not pointed)
5. Sectioning clips (minimum 8)
6. Gloves, cape, barrier cream for hairline
7. Timer (phone or station clock)
8. Toner supplies: **Shinefinity gloss** in target shade + applicator bottle
9. Aftercare take-home sheet (printed, in station drawer)

**Section 3 — Step-by-Step Protocol**

1. **Consult and confirm.** Review the client's consultation notes (see Client Consultation Notes skill). Confirm target shade, placement preferences, and any contraindications. Estimated time for this step: 5 minutes.

2. **Section the hair into 4 quadrants.** Part from ear to ear and center part from forehead to nape. Clip each section securely.

3. **Begin at the back-bottom quadrant.** Take a 1-inch subsection. *Watch out: starting too thick will result in chunky, unblended highlights.*

4. **Backcombing technique.** Lightly backcomb the root area (first 1-2 inches from the scalp) to create a soft root shadow. The amount of backcombing controls the blend — less backcombing = more root brightness.
   *Checkpoint: the backcomb should feel gentle, not aggressive. If you're hearing a crunching sound, you're backcombing too hard.*

5. **Apply lightener.** Load the brush and paint from the mid-lengths to the ends first, then feather upward toward the backcombed area. The saturation should be heavier at the ends and lighter toward the roots.
   *Sensory cue: the consistency of the lightener on the brush should be like thick yogurt — if it's runny, add more powder.*

6. **Place the foil.** Slide a foil strip under the painted section. Fold in half to seal. This prevents the lightener from transferring to unsaturated hair beneath.

7. **Repeat** through all 4 quadrants. Work bottom to top within each quadrant. Focus density on the face frame and crown (where the eye naturally goes).

8. **Set your timer.** Check at **20 minutes**, then every 5 minutes after. Target lift: pale yellow. *Watch out: do NOT rely on time alone — always check the actual lift level by opening a foil.*

9. **Rinse when target lift is reached.** Rinse with lukewarm water. Shampoo with a bonding shampoo (**Olaplex No. 4**). Towel dry.

10. **Apply toner.** Mix **Shinefinity gloss** in the client's target shade. Apply evenly root to tip. Process 20 minutes at room temperature. Rinse and condition.

**Section 4 — Client Communication Script**

At the start: "Today we're doing a balayage, which is a hand-painted highlighting technique. It gives you that natural, sun-kissed look that grows out really beautifully. The whole process takes about 2 to 2.5 hours."

During processing: "I'm going to check your foils in about 20 minutes. While you wait, can I get you anything — water, tea, a magazine?"

At the finish: "Your new color looks beautiful. I'm going to walk you through a few tips for keeping it looking this good at home."

**Section 5 — Aftercare Instructions (Client-Facing Card)**

YOUR BALAYAGE AFTERCARE
- Wait 48 hours before your first shampoo to let the color fully set.
- Use a color-safe, sulfate-free shampoo (ask your stylist for a recommendation).
- Wash with lukewarm water — hot water opens the cuticle and fades color faster.
- Use a purple shampoo 1-2x per week if your shade is blonde, to prevent brassiness.
- Minimize heat styling for the first week. When you do use heat, always apply a heat protectant.
- Your recommended maintenance appointment: 10-12 weeks from today.
- Questions? Call or text us at [salon phone].

**Section 6 — Troubleshooting**

| Issue | Solution |
|-------|----------|
| Uneven lift (spotty results) | Likely inconsistent saturation. Re-apply lightener to under-processed sections. Next time, ensure consistent product loading on the brush. |
| Too warm/brassy after toning | Re-tone with a violet-based gloss (add 10% violet to the formula). Process for the full 20 minutes. |
| Client says "it's not light enough" | Show them the before photo. Explain that balayage is a gradual technique and a second session can add more brightness safely. Never double-process in one sitting. |
| Scalp irritation during processing | Rinse immediately. Apply barrier cream more generously next time. Check if the client had recently scratched or washed their scalp. |
| Foils slipping during processing | Pre-fold a crimp at the top of each foil. Ensure hair is damp enough for the foil to grip. |

**Section 7 — Assessment Checklist**

| Skill | Independently? |
|-------|---------------|
| Sections hair into 4 clean quadrants | Yes / Needs practice |
| Backcombs with appropriate tension | Yes / Needs practice |
| Applies lightener with consistent saturation | Yes / Needs practice |
| Monitors processing and checks lift accurately | Yes / Needs practice |
| Mixes and applies toner correctly | Yes / Needs practice |
| Communicates process clearly to the client | Yes / Needs practice |
| Delivers aftercare instructions | Yes / Needs practice |
| Completes full service within 2.5 hours | Yes / Needs practice |

Minimum to pass: all items marked "Yes." If any item is "Needs practice," schedule one more supervised session before independent client work.

---

**Quick Reference Card (post at station)**

BALAYAGE PROTOCOL — QUICK REF
1. Consult + confirm target shade
2. Section into 4 quadrants
3. Start back-bottom, 1" subsections
4. Backcomb roots gently, paint mid-to-ends
5. Foil each section, work bottom-to-top
6. Focus density on face frame + crown
7. Check at 20 min, then every 5 min — target: pale yellow
8. Rinse, bond shampoo, towel dry
9. Tone with Shinefinity gloss — 20 min
10. Rinse, condition, style, aftercare card
