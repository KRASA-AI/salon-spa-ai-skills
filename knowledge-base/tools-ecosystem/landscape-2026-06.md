# Salon & Spa AI Tools Ecosystem Snapshot — June 2026

*Compiled from the 2026-06-01 landscape monitor run. Reference for skills in this repo when describing the tooling landscape — not a vendor endorsement. Concept-level only; no vendor copy is quoted.*

---

## What Changed Since the May Snapshot

The landscape is largely consistent with the 2026-05 snapshot. The notable shifts in the four weeks since that file are regulatory rather than product, and one fresh tooling category — AI-driven hiring — that crossed from "background trend" into "discrete skill gap" this cycle.

**Regulatory shifts in this window (full detail in `knowledge-base/regulations/state-by-state-med-spa-2026.md`):**
- **California AB 489** is now confirmed effective January 1, 2026 with implications specifically for AI-drafted marketing, social, review-reply, and chatbot copy at any CA-state med spa. The skill `ai-consent-and-compliance-guardrails` was already partially aware of disclosure rules; AB 489 adds a separate hard gate on license-implying terms regardless of whether disclosure is present.
- **Colorado SB 26-189** was signed May 14, 2026, repealing and replacing the prior 2024 Colorado AI Act. Effective January 1, 2027. HIPAA-covered entities are largely exempt from the main deployer duties; for med spas, the practical capture is AI hiring tools and AI financing-eligibility tools, not AI-drafted marketing copy. This reshapes the prior repo guidance, which had treated the 2024 CO AI Act as the primary CO compliance frame.

## Categories of AI Tooling Operators Are Using (June 2026)

### Booking & Scheduling
Platform-native AI booking and waitlist gap-fill are now table stakes across Boulevard, Mangomint, Vagaro, GlossGenius, Zenoti, Meevo, Mindbody, Booksy, and Fresha. Differentiation is at the high end (deeper CRM integration, sentiment detection, treatment-aware intake) and the low end (solo-stylist accessibility). The AI receptionist / voice agent category has now been off the active skill watchlist for ten consecutive cycles — the value-add is platform integration, not prompt design.

### Inventory & Back-Bar
Real-time per-service deduction, AI-driven reorder suggestion, and low-stock alerts are now bundled into most mid-tier and above platforms (SalonScale, Vagaro, GlossGenius, Mangomint, Meevo, Zenoti). Industry reporting suggests salons running per-service deduction see meaningfully less product waste vs. spreadsheet inventory. This is platform-native; a skill in this area would be a weekly "back-bar reorder review brief" that aggregates the platform's recommendations into an owner-readable summary — currently overlaps too heavily with `operations/morning-owner-brief` and `operations/weekly-kpi-owner-briefing` to warrant a separate file. Re-evaluate in Q4 2026 if a clear gap surfaces.

### Client Retention & Win-Back
Predictive analytics for lapse-risk and churn detection continue to mature on the platform side. Industry case data referenced in this cycle suggests rebooking rates can roughly double when predictive rebooking triggers fire seven days before the client's typical rebooking window — and that 50–70% of churn happens before the second visit. The existing `customer-service/client-winback-sequence` skill covers the 60 / 90 / 120-day cadence well; the 7–14-day "missed-expected-visit" window is partially covered by `customer-service/treatment-cadence-rebooking`. A tighter coupling between those two skills — so a missed expected rebook triggers a same-week warm check-in before it ages into a 60-day winback — is a v2.x candidate for the next skill-evaluator cycle.

### Dynamic / Surge Pricing
The category is real but mostly platform-native (Zenoti, Mindbody, Boulevard surface dynamic pricing as a feature). The interesting layer for a prompt skill is the *client communication* around a price change — how to announce dynamic pricing without triggering backlash. This overlaps heavily with the existing `_shared/email-drafter` plus `sales/sms-campaign-builder` and does not warrant a standalone skill. California's October 2025 algorithmic-pricing laws (effective January 1, 2026) are mostly about shared-algorithm collusion, not single-business dynamic pricing — flagged for awareness, not action.

### AI Marketing & Content
Personalized social, summer / seasonal email, AI image generation for promo creative, and AI ad-copy variations are all now standard across the trade-press recommendations. The skill `sales/social-content-calendar-planner` plus `_shared/email-drafter` plus `sales/sms-campaign-builder` cover the practical operator needs here. New-this-cycle observation: vendor prompt packs ("200 AI prompts for stylists" style) continue to proliferate and continue to be uneven in quality. The skill-design discipline remains "build from the workflow up, never paraphrase a prompt pack."

### Birthday / Anniversary / Milestone Marketing
Now bundled into most loyalty and CRM platforms. The existing `sales/loyalty-program-builder` and `sales/sms-campaign-builder` cover the operator-side need; no new skill is warranted.

### Staff & Hiring (new this cycle)
This is the category that crossed the action threshold this run. Trade-press reporting consistent across multiple sources: stylist departure costs $7,500–$15,000 per role; structured interviews are roughly 3x more predictive of performance than unstructured; apprentice-trained stylists retain longer. Tooling on the AI-driven hiring side (resume scoring, automated screening) is emerging on the platform side but raises a discrete compliance question in some states — Colorado SB 26-189's deployer duties for "consequential decisions" capture AI hiring tools after 2027-01-01 for non-HIPAA-covered entities. The skill `operations/stylist-hiring-rubric` was created this cycle to fill the structured-rubric gap; the AI-screening-tool question is flagged as a compliance gate inside the skill rather than as a separate file.

### Review Response
Already covered by `customer-service/review-response-writer` plus `_shared/review-responder`. The CA AB 489 impact (no "clinician-approved" or "doctor-recommended" language without supervising-licensee sign-off) is now flagged in the state KB and routes into the review-response skill's CA-state gate.

### Provider / Practitioner Branded Content
Personal-brand content for individual stylists or providers (TikTok-style work walkthroughs, before/after carousels, niche-expertise short-form) is platform-native to the creator tools and not a skill gap. The supporting copy work (captions, retail product positioning, membership cross-sell within personal content) is covered by `sales/social-caption-writer` and `sales/retail-product-recommender`.

### AI Receptionist / Voice Agent
**Off active watch — 10th consecutive cycle.** Continues to be a platform-integration category rather than a prompt-skill category. Re-evaluate only if a meaningful operator-tunable script artifact emerges.

### Aesthetic Tech & Med-Spa AI (upstream brand / formulation side)
Continues to be product-brand-side, not operator-side. Reference for context; not actionable for this repo's audience.

---

## Active Watch (Carried Forward)

- **EU AI Act Article 50 Code of Practice** — final version expected in early June 2026 (around this monitor cycle's date). Applicability date August 2, 2026. The v1.1 review of `operations/ai-consent-and-compliance-guardrails` Artifact 4 (EU booking-confirmation notice) remains scheduled for the July 2026 monitor cycle, once the final Code is published.
- **Florida SB 1728 / HB 1429** — effective July 1, 2026 if passed. Status to be re-verified at the July monitor cycle.
- **Colorado SB 130** — committee status unchanged at this cycle. Re-check next cycle.
- **Washington Department of Health** — rule review for injectables, microneedling, IV hydration, energy-based devices. No new status at this cycle.
- **Indiana SB 282** — registration deadline 2027-01-01 (med-spa registration). Status unchanged at this cycle.
- **New York 2026 enforcement wave** — 223 inspections / 87 cited as of the May snapshot; the next quarterly enforcement-report read is expected in Q3 2026.

---

## Off Active Watch

- AI receptionist / voice agent (10th cycle).
- AI hair color try-on / virtual visualization (still a consultation aid, not a workflow skill on its own).
- Upstream beauty-brand AI (formulation safety, virtual try-on at the brand-product level).
- Commercial "200+ prompts for stylists" packs (commercial, vendor-paraphrased, low novelty).

---

## How This File Is Used

Skills should reference this file when:
- Describing the tooling landscape in a worked example or a "things to verify" line.
- Deciding whether a new skill is warranted or whether the platform side covers the operator need.
- Flagging a category as platform-native rather than prompt-native, so the skill catalog stays focused on prompt-skill leverage points.

Refresh cadence: monthly snapshot at each landscape monitor cycle. Next file: `landscape-2026-07.md`.
