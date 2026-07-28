---
name: AI Consent & Compliance Guardrails for Client Communications
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/policy update"
version: 1.4
last_eval_score: 9.1
---

# AI Consent & Compliance Guardrails for Client Communications

## Purpose
Generate the four consent, disclosure, and authorization artifacts that a salon, spa, or med spa needs when AI is drafting, routing, or analyzing client communications: (1) an AI-disclosure clause for intake forms, (2) a HIPAA-compliant marketing authorization separating clinical records from marketing outreach, (3) a before/after photo authorization scoped per platform and revocable, and (4) an EU AI Act Article 50 disclosure pack scoped to the **final Art. 50 guidelines (published 2026-07-20)** — the bot must reveal it is AI at first interaction (a duty the guidelines assign to the bot's **provider/vendor**; the deployer-salon's job is to confirm the vendor shipped it enabled and not disable it), plus the salon's own narrow **deployer** labelling duties (deepfakes + public-interest text only) — for any salon that serves EU clients. Also produce a human-in-the-loop review checklist that defines exactly which AI drafts need a licensed provider's sign-off before they go out, including the CA AB-489 license-language gate and the CA SB 351 consultation gate where those states apply.

This skill operates at the policy layer — it is the guardrail that the downstream message skills (`review-response-writer`, `treatment-cadence-rebooking`, `sms-campaign-builder`, `social-caption-writer`, `social-content-calendar-planner`, `retail-product-recommender`) all draw from. It is not a replacement for licensed legal counsel; it is a first-draft generator that a salon's attorney and medical director can then mark up.

## Quick Start — Which Artifacts Do I Need?

This skill can produce five deliverables, but most practices need only two or three. Answer two questions, generate only what your row calls for, and skip the rest of the gate detail below unless it applies to you.

**Q1 — What kind of business are you?** salon / day-spa (no medical services) · med-spa (any injectable, laser, RF, IPL, IV, peptide, or other clinical-tier service).
**Q2 — Do you serve any EU clients or have EU-based staff?** yes · no.

| Your situation | Generate these | Skip |
|---|---|---|
| **Salon / day-spa, no EU** | Artifact 1 (AI disclosure), Artifact 2 as the **simple TCPA/GDPR opt-in** (no HIPAA authorization), Review Checklist (items 1–8) | Artifact 4; the HIPAA-authorization form; the CA clinical gates unless you sell clinical-tier services; Artifact 3 only if you post client photos |
| **Salon / day-spa, serves EU** | The row above **plus Artifact 4** (chatbot-disclosure line + privacy paragraph) | HIPAA authorization; clinical gates |
| **Med-spa, no EU** | Artifacts 1, 2 (full HIPAA marketing authorization), 3 (if you post before/after photos), full Review Checklist (items 1–10) | Artifact 4 |
| **Med-spa, serves EU** | **All four artifacts** + full Review Checklist | nothing — this is the full pack |

**State gates fire on top of the above, only for the states you operate in:** CA → AB-489 language gate + SB 351 consultation gate (both apply to clinical-tier copy); CO → SB 26-189 (only if you use AI for hiring or financing decisions; effective 2027-01-01); IN → SB 282 registration; others per `state-by-state-med-spa-2026.md`. If you are not in CA/CO/IN/NY/OH/TX/IA, you can skip the State & EU Regulatory Gates section entirely and still get a complete pack. Read the full gate block below **only** for the states you actually operate in.

Everything past this point is the detailed reference the drafter uses to build the artifacts your row named. A salon with no EU exposure and no clinical services never needs to read the CA/CO/EU gate detail at all.

## When to Use
- A med spa is rolling out AI-drafted SMS, email, or voice-agent outreach and needs consent infrastructure before launch.
- An existing client intake form was written before AI tooling was in place and has no AI disclosure language.
- The practice is preparing for an audit, a BAA renewal, or a new HIPAA risk assessment and needs its consent pack refreshed.
- Social media before/after posts have been going out under a generic intake consent — the team needs a per-platform, revocable authorization.
- The salon has EU clients or EU-based staff and wants to get ahead of the August 2026 EU AI Act transparency deadline.
- A staff member flagged a near-miss — an AI-drafted message almost went out with a treatment claim, a dose reference, or a named adverse event — and the practice needs a written review gate.

## Required Input
- **Business profile**: salon / day spa / med spa (MD-owned, NP-led, or RN-led), scope of medical services, state(s) of operation, whether any EU clients are served, whether the practice holds a Business Associate relationship with any AI vendor.
- **AI tools in use**: drafting assistants (Claude / ChatGPT), voice receptionist (TrueLark, Qlient, AgentZap, Fresha AI receptionist, etc.), intake-form builder (Jotform AI Agent, GlossGenius Genius Forms, etc.), charting assistant, marketing platform. List which data each tool touches.
- **Channels in use**: SMS, email, in-app, phone, social DMs, voice.
- **Medical director / licensed provider names** for the attribution lines and the review gate.
- **Existing policies to preserve**: cancellation, refund, photo, privacy, opt-out — so the new language plugs in rather than contradicts.
- **Known constraints**: any state scope-of-practice rules, any pending litigation or patient complaint, any manufacturer compliance notices (Allergan, Galderma, etc.) the practice has received.

## Instructions

You are a salon/med-spa compliance author. You write in plain, non-legalistic English, but you never drop an element that a HIPAA auditor, a state medical board investigator, or an EU AI Act regulator would look for. You cite the category of authority behind each clause (HIPAA marketing authorization, TCPA consent, GDPR lawful basis, EU AI Act Article 50 transparency, state cosmetology regulation, FTC endorsement guides) but do not quote statute text verbatim. You do not offer legal advice — you offer a first draft that the practice's attorney and medical director will review.

Load business context from `config.yml`. Reference `knowledge-base/terminology/` for service and product names, and `knowledge-base/policies/` for any existing policy snippets to preserve. Reference `knowledge-base/regulations/state-by-state-med-spa-2026.md` for the live status of every state and EU gate named below — cite by state/rule name, never quote statute text.

### Config Integration — What To Pull From `config.yml`

This skill is the policy layer; its output must name the practice's real medical director, real states of operation, and real tool stack, or it is generic boilerplate the attorney throws out. Map every key to a per-key fallback.

| Config key | How the skill uses it | If absent |
|---|---|---|
| `business.name` | Clinic name across all four artifacts and the checklist header. | Use [Clinic Name] placeholder and flag for fill. |
| `business.business_type` | Branches the output: `salon` / `day-spa` → simpler TCPA/GDPR opt-in (no HIPAA marketing authorization); `med-spa-*` → full HIPAA marketing authorization + clinical review gate. | Default to the conservative med-spa branch and flag "confirm business type." |
| `business.location.state` (may be a list) | Selects which state gates fire (see State & EU Regulatory Gates below): CA → AB-489 + SB 351; CO → SB 26-189 + HB 1024; NY/OH/TX → ownership/scope hard gates; IA → supervision-hours; IN → SB 282 registration; etc. | Fire no state gate; flag "state(s) of operation not configured — state compliance gates not applied." |
| `business.serves_eu_clients` (bool) | Turns the EU Art. 50 Artifact 4 on/off and scopes it. | Default off; note Artifact 4 is omitted unless EU clients are served. |
| `business.voice.tone` | Plain-language register of the client-facing artifacts (1–3). | Default: warm, plain, non-legalistic. |
| `services.cadence_class` / `services.menu` | Identifies which services are clinical-tier (`med-spa-*`) and therefore trigger the SB 351 consultation gate and the prescription-product checklist items. | Treat any service with clinical keywords (neuromod, filler, laser, IPL, RF, microneedling, IV, peptide, compounded) as clinical-tier and flag for confirmation. |
| `compliance.medical_director` (name + credential) | Attribution lines and the review-gate sign-off; the SB 351 PSO/GFE authority. | Insert `— MEDICAL DIRECTOR NOT CONFIGURED —` rather than a generic "your provider"; this is a hard flag for med-spa output. |
| `compliance.scope_of_practice` | Who may legally perform each service (drives the review-checklist scope item). | Skip the scope line and flag "scope-of-practice map not configured — verify before any provider attribution." |
| `compliance.ai_tools[]` (name + data each touches + BAA status) | Names the tools in Artifact 1's data-category disclosure and the BAA table-stakes check. | List "[AI tools in use]" and flag for fill; cannot confirm BAA coverage without it. |
| `compliance.existing_policies` | Plug-in points so new language doesn't contradict cancellation/refund/photo/privacy policies already in force. | Note "existing policies not supplied — verify no conflict before publishing." |

### State & EU Regulatory Gates (read before drafting)

Before generating any artifact, resolve `business.location.state` and `business.serves_eu_clients` against `knowledge-base/regulations/state-by-state-med-spa-2026.md` and apply the gates that fire. These are **hard gates** — they change the copy, not just a footnote. Three are new in this version (v1.1).

**CA AB-489 — license-implication language (hard gate, effective 2026-01-01).** For any CA practice, AI-drafted client-facing or marketing copy that touches a clinical service may **not** use terms, post-nominal letters, or design elements implying the *speaker* is or has the qualifications of a licensed health-care professional unless the content is genuinely supervised by an in-state licensee. Restricted registers include "doctor-level," "clinician-guided," "MD-formulated," "expert-backed," "medically supervised" (when no supervision occurred for that copy), and lookalike credential letters. **Each use of a restricted term is a separate violation.** Two operator-facing consequences this version bakes in: (1) the Review Checklist gains an explicit AB-489 line; (2) because boards are running automated **"crawler audits"** of a practice's *standing* public surface — website service pages, evergreen social captions, AI-chatbot fallback scripts — the gate applies to **persistent copy, not just one-off campaigns**. An AI receptionist/chatbot fallback that says "our medical experts recommend…" is a standing AB-489 exposure even if no individual message was ever "sent."

**CA SB 351 — Good-Faith-Exam / Patient-Specific-Order consultation gate (2026 enforcement standard).** For CA clinical-tier services (injectables, lasers, RF, IPL, IV, peptides, compounded meds), no AI-drafted intake, booking-confirmation, or marketing copy may promise, pre-approve, or imply same-visit treatment delivery. Treatment is conditioned on a documented Good Faith Exam and an individualized **Patient-Specific Order** issued by the responsible clinician. This is a **consultation-flow** gate distinct from the AB-489 *language* gate; the two stack. Upstream owners of the live consultation flow are `client-consultation-intake` / `virtual-consultation-intake`; this skill enforces the no-pre-approval principle in the policy artifacts and the review checklist.

**CO SB 26-189 — AI-deployer duty, HIPAA-covered-vs-not (signed 2026-05-14; effective 2027-01-01).** Replaces the 2024 Colorado AI Act. HIPAA-covered entities and their business associates are **largely exempt** from the main deployer duties — **except** for consequential employment and financial-assistance decisions. The capture for a CO med spa is therefore **not** AI-drafted marketing copy; it is AI tools that materially influence a consequential decision — most likely **AI-driven hiring** (`stylist-hiring-rubric` if automated) and **AI-driven financing-eligibility** tools. Do not assume a CO practice is automatically exempt: a pure cosmetic-only spa with no medical records may not be HIPAA-covered and may carry deployer documentation/risk-management duties for those two tool categories. Note the 2027-01-01 effective date as a build-toward, not a current obligation.

**EU AI Act Article 50 (final Code of Practice published 2026-06-10; final Art. 50 guidelines published 2026-07-20).** See Artifact 4 below for the full scoped treatment. The headline, corrected against the final guidelines: an EU-client-facing AI booking bot must reveal it is AI at first interaction (**Art. 50(1)**), but that duty is a **provider** obligation — it binds the bot **vendor**, not the deployer-salon running a third-party bot. The salon's job is to **confirm the vendor's bot ships that disclosure enabled and not disable it** (and to know that white-labelling or self-building a bot pulls the provider duty onto the salon). The salon's *own* direct **deployer** duties are the narrower **50(3)** (emotion-recognition/biometric-categorisation notice) and **50(4)** labelling (deepfakes + public-interest text only — *not* routine booking/marketing copy; human-reviewed text is exempt). Do not over-label. One med-spa-specific 50(4) exposure: AI-simulated **before/after** imagery resembling a real, identifiable person (see Artifact 4).

### The Four Artifacts

**Artifact 1 — AI Disclosure Clause for the Intake Form**

A short paragraph the client reads and checkboxes during digital intake. It must cover:
- That AI tools may help draft communications, summarize consultation notes, analyze submitted photos, or route inquiries.
- What categories of data the AI tools access (contact info, appointment history, treatment notes, photo submissions).
- That AI-generated drafts are reviewed by a human staff member before any clinical decision is made.
- How the client can opt out of AI-assisted analysis (and what still works if they do — e.g., they can still book, but their intake will be reviewed by a human only).
- A contact email for consent-related questions.

Keep to 120–180 words. Plain language. Active voice. No "the Company" or "the Undersigned."

**Artifact 2 — HIPAA-Style Marketing Authorization (channel-specific)**

A separate form from the general treatment consent, required for med spas that handle Protected Health Information. Must:
- Be a standalone signed authorization, not a checkbox buried in the intake.
- Enumerate the specific marketing uses authorized (SMS offers, email newsletters, birthday messages, retail product recommendations based on treatment history, AI-generated personalized offers).
- Make the channel grant explicit per channel — a client may say yes to email, no to SMS.
- Include an expiration date (suggest 24 months) and a revocation method (reply STOP, email, in-app).
- Name the categories of PHI that may be used to personalize marketing (service history, treatment category, not diagnosis or clinical notes).
- State that treatment is not conditioned on signing.
- **Name AI-generated voice specifically if the practice runs (or plans to run) an outbound AI voice agent.** US telephone-consumer rules treat AI-generated voice as an **artificial voice**, which requires prior express *written* consent for marketing calls, with per-call statutory damages and no aggregate cap — and the *proposed* federal standard would require the consent language to **name AI use specifically**. A generic "you may receive automated calls" line may not carry the load. So the opt-in must have a **distinct, separately-checked** grant for AI-generated voice calls before any outbound voice agent is switched on. This is a **hard dependency for `operations/ai-receptionist-disclosure-and-escalation`** (its outbound block will not clear go-live until this opt-in exists). Do not fold it silently into the general "phone" or "automated messages" grant.

For non-medical salons: produce a simpler opt-in that still meets TCPA (for SMS) and GDPR (for EU clients) — explicit affirmative consent, clear purpose, right to withdraw — and, if an outbound AI voice agent is in play, the same distinct AI-voice grant described above.

**Artifact 3 — Before/After Photo Authorization (per platform, revocable)**

A separate form for any image that will appear on social or marketing channels. Must:
- Name the specific platforms (Instagram feed, Reels, Stories, TikTok, Facebook, Pinterest, website gallery, email nurture, case-study deck). Client can grant or withhold per platform.
- State whether the face is cropped / blurred / fully visible.
- Disclose any AI editing (color correction, lighting normalization, retouching) and require a second checkbox if retouching beyond white-balance is intended.
- State the authorization duration and a one-line revocation path ("reply or email CONSENT-REVOKE to [address] and we will remove within 30 days from all channels we control").
- Include the FTC endorsement disclosure if the client is being compensated with services, discounts, or product in exchange for the image.

**Artifact 4 — EU AI Act Article 50 Disclosure (chatbot-first, scoped)**

Only generate this artifact if `business.serves_eu_clients` is true (or the practice has EU-based staff/clients on file). Produce it in the scope the **final Art. 50 guidelines (published 2026-07-20)** and the **final Code of Practice (published 2026-06-10)** actually require — and not one inch beyond, because over-labelling routine copy is its own failure mode this version corrects.

What the final guidelines and Code mean for a salon/spa/med-spa **deployer** — and this version **corrects a duty-ownership error** the earlier version carried:

- **Chatbot disclosure — Art. 50(1) — is a PROVIDER duty, not the salon's.** An EU-facing AI booking bot, receptionist, or conversational scheduler must tell the client, clearly and at first interaction, that they are talking to AI. But the final guidelines (*Guidelines on transparency obligations for providers and deployers of AI systems*, 2026-07-20) are explicit that **Art. 50(1) binds the provider** — whoever develops the bot and places it on the market under their own name/trademark, i.e. the **bot vendor** (Fresha, TrueLark, Qlient, etc.) — **not the salon that switches a vendor's bot on.** A salon running a third-party bot is a **deployer**. So the salon's operative task is: **confirm the vendor's bot ships the "you're talking to AI" disclosure enabled, at first interaction, and don't disable it.** Two carve-outs: (a) a salon that **white-labels, self-brands, or self-builds** a bot under its own name **becomes the provider** and owns the 50(1) duty directly; (b) the disclosure must land **at the moment of first interaction** — not buried in ToS, a settings menu, or a privacy policy — and the duty **extends to "agentic" systems** that book/reschedule/cancel on the client's behalf, not only passive Q&A bots. The "obvious it's a bot" exception is read **restrictively** (a reasonably-informed, observant average person). *This is the correction: the earlier version called 50(1) "the primary operator duty," which is accurate about what the bot must do but wrong about who the Act names as legally responsible — the failure mode the repo watches for after the TX SB 140 error.* Still draft the disclosure line below, because the salon needs the language to verify (or, if self-built, to own) it.
- **The salon's OWN direct deployer duties — Art. 50(3) and 50(4).** **50(3):** if the practice runs an emotion-recognition or biometric-categorisation system (rare for a salon — e.g. an AI "skin-emotion" or face-analysis tool), it must inform the people exposed to it. **50(4) labelling:** applies to **deepfakes** and to AI-generated/manipulated **text published to inform the public on matters of public interest**. Ordinary AI-drafted booking confirmations, promo emails, SMS, and intake copy are **outside** that public-interest trigger, and **human-reviewed text is exempt regardless**. **Do not** stamp a blanket "this message was written by AI" label on routine transactional or marketing copy — the guidelines do not require it, and doing so trains clients to distrust normal comms.
- **Med-spa 50(4) deepfake flag (new this version).** AI-generated or AI-**simulated before/after** imagery that resembles a real, identifiable person and could be taken as an authentic result photo can meet the **deepfake** definition. When it does, the **deployer** (the practice) must disclose/label it on first exposure and **cannot rely on the provider's machine-readable mark alone** to satisfy 50(4). This is the one routine-marketing surface where a salon's own 50(4) duty actually bites — cross-reference Artifact 3 (photo authorization), which is where AI-editing of client imagery is already disclosed.
- **Provider duty, not yours — Art. 50(2).** Machine-readable provenance marking of AI **audio/image/video/text** is the **provider's** obligation (the tool vendor — Fresha, TrueLark, an AI image generator). The salon does not hand-stamp these marks. If marketing uses AI-generated images, the provenance mark is set upstream by the image tool. (Note the med-spa 50(4) exception above: for a before/after deepfake, the provider's 50(2) mark does **not** discharge the deployer's 50(4) label.)
- **Optional EU label icon (Annex I).** The Code offers an optional EU label icon in three variants. It is **available but not required**. Mention it as an option a brand may adopt for trust signalling; never present it as mandatory.
- **Timeline — no grace on the duties that matter here.** Art. 50(1), 50(3), and 50(4) are **live 2026-08-02 with no grace period**. The transitional period to **2026-12-02** applies **only** to the Art. 50(2) machine-marking duty, and **only** for systems already on the market before 2026-08-02 — it does **not** buy the salon extra time on chatbot disclosure or deepfake labelling. Fines run to **€15M or 3% of worldwide turnover**, whichever is greater, with **SME/small-mid-cap proportionality** — a small practice sits in the proportionality band. Enforcement is by national market-surveillance authorities. The **2026-07-22 signatory-form date** for the (separate) Code of Practice is a *signatory* deadline, not an operator compliance deadline — a small deployer generally need not act on it.

Deliverables:
1. A **chatbot first-interaction disclosure line** (the Art. 50(1) text) — one sentence the AI booking/receptionist bot shows at the start of an EU-client conversation. Frame it as the language the salon **verifies its vendor uses** (or owns directly if it self-built the bot), not as a label the salon bolts on.
2. A **one-sentence booking-confirmation append** (Touch 1 in `booking-confirmation-sequence`) noting the booking was AI-assisted and human-reviewed — kept light, *not* framed as a mandatory 50(4) label.
3. A **privacy-page paragraph** the confirmation links to, covering data accessed, the human-review step, the opt-out-to-human path, and the 2026-08-02 applicability with the 2026-12-02 transitional cutoff for the 50(2) marking duty only.
4. If the practice posts AI-edited or AI-simulated **before/after** imagery to EU-facing channels, a **one-line deepfake label** for first exposure (the med-spa 50(4) duty above) — routed through Artifact 3.

### Human-in-the-Loop Review Checklist

A one-page gate the practice staples to the AI-drafting workflow. Every outbound AI draft is checked against it before it can be approved to send. The licensed provider or a designated, trained staff member signs off. Items to include:

- Does the draft contain any clinical claim, treatment-outcome promise, or before/after percentage that is not in the practice's pre-approved claim library?
- Does the draft name a prescription product (Botox, Dysport, Daxxify, Restylane, Juvederm, etc.) in a promotional way that could trigger manufacturer compliance flags?
- **(CA AB-489)** Does the draft use any term, post-nominal letters, or design element implying the speaker is or has the qualifications of a licensed health-care professional ("doctor-level," "clinician-guided," "MD-formulated," "expert-backed," "medically supervised") without genuine in-state licensed supervision for *this* copy? Each restricted term is a separate violation. Applies to **standing copy too** — website service pages, evergreen captions, chatbot fallback scripts — not just the message in hand.
- **(CA SB 351)** For a CA clinical-tier service, does the draft promise, pre-approve, schedule-to-treat, or imply same-visit treatment without preserving the "subject to a Good Faith Exam and a Patient-Specific Order" condition? Pre-approval language is a publish-blocker.
- Does the draft reveal a client's diagnosis, medication, pregnancy status, or any adverse event?
- Is the draft going to a client whose channel-specific marketing authorization is on file and not expired?
- For AI-assisted image posts: is the before/after authorization for this platform on file and not revoked?
- For new-client AI outreach: has the AI-disclosure clause been acknowledged on the intake?
- Does the draft contain a working opt-out mechanism appropriate to the channel (STOP for SMS, one-click unsubscribe for email, revocation contact for voice)?
- If a complaint, refund, or adverse-event topic is implicated: has the message been escalated off the AI-drafting path entirely and routed to a human-only response?

### Design Principles

1. **Separate marketing authorization from treatment consent.** A client signing a treatment consent on the day of service is not the same as a client authorizing marketing. Keep them as distinct documents with distinct signatures.
2. **Make consent granular by channel and platform.** A single "yes to marketing" checkbox is increasingly insufficient under TCPA, GDPR, and the EU AI Act. The client picks the lanes.
3. **Make revocation as easy as consent.** If it is easier to sign up than to leave, regulators view that as coercive.
4. **Never AI-draft a reply to a complaint, refund request, adverse-event mention, or injury allegation.** Those conversations route to a licensed human full-stop — this is enforced by the review checklist.
5. **Treat the AI-vendor BAA as table stakes.** If a tool touches PHI and no Business Associate Agreement is in place, it cannot be in the loop for med-spa communications.
6. **Mirror the EU AI Act timeline — and scope the duty to the right party.** Article 50 applicability (2026-08-02, with the 2026-12-02 transitional cutoff applying **only** to the 50(2) marking duty for pre-existing systems) is a fixed calendar event; build toward it now. But scope the duty correctly: **chatbot disclosure (Art. 50(1)) is the bot vendor's (provider's) obligation** — the salon's job is to verify it ships enabled and not disable it, unless the salon self-builds and becomes the provider. The salon's *own* deployer duties are 50(3) (emotion/biometric notice) and the **narrow** 50(4) labelling (deepfakes + public-interest text; human-reviewed text exempt), including the med-spa AI-simulated before/after deepfake case. Over-labelling routine copy is a failure mode, not caution.
7. **Treat license-implication language as a separate hard gate (CA AB-489).** Whether copy is a campaign or a permanent website line, an AI system may not imply licensed-professional qualifications it does not genuinely have. Each use is a separate violation, and boards audit the *standing* public surface — so the gate runs on evergreen copy and chatbot fallbacks, not just sends.
8. **Never get ahead of the clinical gate (CA SB 351).** AI copy for a clinical-tier service must never pre-approve or schedule-to-treat; treatment is conditioned on a documented Good Faith Exam and an individualized Patient-Specific Order.

### Output Format

Return all five deliverables in this order, each in its own clearly labeled section:

1. **Artifact 1 — AI Disclosure Clause** (120–180 words, ready to paste into the intake form).
2. **Artifact 2 — Marketing Authorization** (full form with signature, date, channel checkboxes, expiration, revocation method).
3. **Artifact 3 — Photo Authorization** (full form with per-platform grants, retouch disclosure, revocation path, FTC disclosure if applicable).
4. **Artifact 4 — EU AI Act Booking Notice** (chatbot first-interaction line to verify with the vendor + one-sentence confirmation append + linked privacy paragraph + a before/after deepfake label if AI-simulated result imagery is posted to EU-facing channels).
5. **Review Checklist** (one page, scannable, with a signature and date line at the bottom).

Beneath each artifact, include a short **rationale block** noting:
- Which regulatory frame this addresses (HIPAA marketing, TCPA, GDPR, EU AI Act Article 50, FTC endorsement guides, state scope of practice).
- What this draft does not do (e.g., "does not substitute for legal review," "does not cover sensitive category data beyond treatment history").
- One or two edge cases for the practice's attorney to focus on.

## Example Output

**Practice profile:** MD-owned med spa in California; services include Botox, filler, laser hair removal, HydraFacial, medical skincare. Five EU clients on file. Uses Claude for drafting SMS and email; uses Fresha for booking with its AI receptionist; uses Jotform AI Agent for intake. Medical Director: Dr. Priya Chen, MD.

---

**Artifact 1 — AI Disclosure Clause (intake form, checkbox)**

At [Clinic Name], we use AI tools to help our team work faster and keep your experience consistent. These tools may draft appointment messages, summarize the notes we take at your visit, help analyze photos you send us, and route your questions to the right provider. Every AI-drafted message is reviewed by a trained staff member before it reaches you, and every treatment decision is made by your licensed provider — not by an AI.

The tools we use today touch your contact information, appointment history, treatment category, and any photos you choose to upload. They do not make marketing decisions on their own.

If you would prefer that a human review your intake without AI assistance, check the box below. You can still book, and we will still personalize your care — it will just take us a little longer. Questions? Email privacy@[clinicdomain].

☐ I acknowledge the above.
☐ I prefer a human-only review of my intake.

*Rationale:* Satisfies the AI-disclosure expectation increasingly cited by state privacy regulators and industry guidance; aligns with EU AI Act Article 50 transparency for EU clients; preserves the practice's ability to use AI drafting while giving the client a real opt-out. Does not substitute for a full Notice of Privacy Practices. Attorney focus: confirm this clause does not conflict with California CCPA/CPRA sensitive-category data handling.

---

**Artifact 2 — Marketing Authorization (separate signed form)**

**Authorization for Marketing Communications — [Clinic Name]**

I authorize [Clinic Name] to send me marketing communications using my contact information and my service history. I understand that my treatment is not conditioned on signing this form, and I can withdraw this authorization at any time.

I grant permission for the following channels (check each one you want):

☐ Email — appointment-adjacent offers, product recommendations, newsletters
☐ SMS — time-sensitive offers, birthday messages, waitlist gap-fill notices
☐ Phone — only if I have reached out first or if there is an appointment-related matter
☐ In-app push notifications
☐ Personalized offers drafted with AI assistance (these are reviewed by a human before sending)
☐ Automated marketing calls or voicemails using an **AI-generated (artificial) voice** — I understand this is a distinct consent, separate from live-agent calls, and I can withdraw it at any time

[Clinic Name] may use the following categories of my information to personalize what I receive: appointment history, service category received, product purchases, and stated preferences. [Clinic Name] will not use my specific diagnosis, medication list, or clinical note content for marketing.

This authorization expires 24 months from the date below. I can revoke it at any time by replying STOP (SMS), clicking "unsubscribe" (email), or emailing privacy@[clinicdomain]. Revocation takes effect within 10 business days.

Signature: __________________________ Date: __________

*Rationale:* Meets HIPAA marketing-authorization requirements (separate form, enumerated uses, non-conditioned, revocable, expiring); layers TCPA per-channel express written consent for SMS; adds a **distinct AI-generated-voice grant** so the practice can lawfully switch on an outbound AI voice agent (artificial-voice marketing requires prior express written consent, and the proposed federal standard would require naming AI specifically) — this is the opt-in that `ai-receptionist-disclosure-and-escalation` treats as a hard go-live dependency; provides GDPR-compatible lawful basis for EU clients. Does not cover employee/vendor data. Attorney focus: confirm 24-month expiration aligns with the practice's internal re-consent cadence and with California's evolving data-broker rules, and confirm the AI-voice grant language satisfies the practice's state's artificial-voice consent standard before enabling outbound voice.

---

**Artifact 3 — Before/After Photo Authorization**

**Before/After Photo Authorization — [Clinic Name]**

I, [Client Name], authorize [Clinic Name] to use the photos described below for the platforms I check. I understand that this authorization is separate from my treatment consent and is not required to receive care.

**Photo description:** [date, service, provider, face cropped / partial / full]

**Platforms authorized (check each one):**

☐ Instagram feed posts
☐ Instagram Reels and Stories (ephemeral content)
☐ TikTok
☐ Facebook
☐ Pinterest
☐ [Clinic Name] website gallery
☐ Email newsletter
☐ Internal case-study or training deck only

**Editing acknowledgment:**
☐ Standard color and lighting correction is permitted.
☐ Retouching beyond color/lighting (smoothing, reshaping, AI-based enhancement) is permitted only if I also check this box.

**Duration and revocation:**
This authorization is valid for 24 months. I can revoke it at any time by emailing photo-revoke@[clinicdomain]. Upon revocation, [Clinic Name] will remove my image from platforms it directly controls within 30 days. Content already redistributed by third parties (re-shares, screenshots) is outside [Clinic Name]'s control.

**FTC endorsement disclosure (if applicable):**
☐ I am receiving a service discount, complimentary service, or product in exchange for permission to use this image. [Clinic Name] will include the tag #ad or #sponsored where required.

Signature: __________________________ Date: __________

*Rationale:* Meets HIPAA's requirement that med-spa before/after photos require a separate authorization specifically naming social media use and each platform; gives the client a per-platform and per-edit-type choice; covers FTC endorsement-guide exposure when services are bartered for content; includes a practical revocation path. Does not cover images a third party might repost. Attorney focus: review the "third-party redistribution" language for the practice's specific risk tolerance.

---

**Artifact 4 — EU AI Act Article 50 Disclosure (this practice serves 5 EU clients, so it fires)**

*1 — Chatbot first-interaction line (Art. 50(1) — verify your vendor uses this; own it if you self-built the bot):*
"Hi! You're chatting with [Clinic Name]'s AI booking assistant. I can answer questions and hold appointment times; a team member reviews every booking, and you can ask for a person anytime."

*2 — Booking-confirmation append (Touch 1 — light, not a mandatory label):*
"This booking was arranged with help from our AI scheduling assistant and reviewed by our team. How we use AI: [clinicdomain]/privacy#ai."

*3 — Privacy-page paragraph:*
"We use an AI scheduling assistant to answer inquiries, suggest appointment times, and draft confirmation messages. It accesses your name, contact details, service interest, and appointment history. It does not make treatment decisions — a trained team member reviews its drafts before they reach you, and a licensed provider makes every clinical decision. You can opt out of AI-assisted scheduling by calling or emailing us for a human-only path. For clients in the EU: our booking assistant discloses that it is AI at the start of every conversation, in line with the EU AI Act. These transparency rules apply from August 2, 2026."

*4 — Before/after deepfake label (only if AI-simulated result imagery is posted to EU-facing channels):*
"This image was digitally simulated to illustrate a possible result and is not an unaltered photo of an actual client."

*Rationale:* The **Art. 50(1) chatbot-disclosure** duty is owned by this practice's **Fresha AI receptionist vendor (the provider)**, not the clinic — so the operative action here is **line 1 is what the clinic verifies Fresha actually shows at first interaction, and does not disable**; the clinic would only *own* this duty if it self-built or white-labelled a bot under its own name. The confirmation append is **deliberately light** — routine AI-drafted booking copy is **not** subject to the narrow Art. 50(4) labelling duty (deepfakes + public-interest text only; human-reviewed text is exempt), so it is not stamped "written by AI." Line 4 exists because AI-**simulated before/after** imagery resembling a real person is the one 50(4) surface a med spa actually touches, and the clinic (as **deployer**) owns that label — the image tool's Art. 50(2) provenance mark does not discharge it. The optional EU label icon (Annex I) is available if the brand wants the trust signal but is not used here because it is not required. Note the 2026-12-02 transition applies only to the 50(2) marking duty, not to 50(1)/(3)/(4), so nothing here gets extra runway. Does not constitute a full AI System Impact Assessment. Attorney focus: confirm Fresha's bot ships the 50(1) disclosure enabled (get it in writing from the vendor); confirm whether any AI face/skin-analysis tool in use triggers the 50(3) emotion/biometric notice; and whether to adopt the optional Annex I icon.

---

**Review Checklist — Pre-Send Gate for AI-Drafted Client Communications**

For every AI-drafted outbound message, the reviewer confirms:

1. ☐ No clinical claim, outcome percentage, or before/after promise beyond the approved claim library.
2. ☐ No promotional reference to a prescription product (Botox, Dysport, Daxxify, Restylane, Juvederm, Sculptra) in a manner that could trigger a manufacturer compliance flag.
3. ☐ No disclosure of diagnosis, medication, pregnancy, allergy, or any adverse event.
4. ☐ Channel-specific marketing authorization on file and not expired for this client.
5. ☐ If the message includes a photo: photo authorization on file, matches the platform, and retouch permissions align.
6. ☐ AI-disclosure clause acknowledged on this client's current intake.
7. ☐ Working opt-out present for this channel (STOP / unsubscribe / contact email).
8. ☐ Topic is not a complaint, refund request, injury mention, or adverse-event question. (If it is, re-route to the medical director's queue — do not send.)
9. ☐ **(CA AB-489 — fires for CA practices)** No term, post-nominal letters, or design element implying licensed-professional qualifications ("doctor-level," "clinician-guided," "MD-formulated," "expert-backed," "medically supervised") without genuine in-state supervision for this copy. Each restricted term = a separate violation. Applies to standing copy too (web service pages, evergreen captions, chatbot fallback scripts) — those are the board crawler-audit targets, not just sends.
10. ☐ **(CA SB 351 — fires for CA clinical-tier services)** Copy does not promise, pre-approve, or imply same-visit treatment; the "subject to a Good Faith Exam + Patient-Specific Order" condition is preserved. For an EU-facing AI chatbot, confirm the Art. 50(1) "you're chatting with AI" disclosure fires at first interaction — this is the **vendor's** duty to build and the practice's duty to **verify and not disable** (unless the practice self-built the bot, in which case it owns the duty). For any AI-simulated before/after image on EU-facing channels, confirm the 50(4) deepfake label is present.

Reviewed and approved by: __________________________ (name, role)
Date and time: __________________________
Message ID or draft reference: __________________________

*Rationale:* Operationalizes the HIPAA "reasonable safeguards" standard, TCPA per-channel consent discipline, FTC endorsement-guide compliance, and Article 50's human-review principle into one pre-send ritual. Defines the bright line (item 8) that an AI draft never gets to cross.

---

**Deployment notes for the practice**

- Store signed authorizations with the client's chart, not in the marketing platform alone.
- Re-prompt authorization at the 24-month mark during a routine visit — do not let it lapse silently.
- Quarterly, pull ten random AI-drafted sends and confirm the review checklist was actually signed for each. This is the audit the practice wants to find its own gaps in, not have a regulator find.
- When adding a new AI tool, re-generate Artifact 1 so the disclosure names the new data categories that tool touches.
- **(CA practices) Run your own AB-489 crawler audit.** Boards are sweeping the *standing* public surface — website service pages, evergreen social captions, AI-chatbot fallback scripts — for license-implication language, so a single-send review is not enough. Quarterly, pass the practice's persistent public copy and every AI-receptionist fallback line through Review Checklist item 9. Fix the standing copy, not just the next campaign.
- **(EU-facing practices) The Art. 50 guidelines are final (published 2026-07-20) — and they reassign the chatbot-disclosure duty.** Art. 50(1) is the **bot vendor's (provider's)** duty, so the standing action is not "write our own disclosure" but **get written confirmation from the booking-bot vendor that its bot discloses it is AI at first interaction, and that the setting cannot be silently disabled by a vendor update.** If the practice self-builds or white-labels a bot, it becomes the provider and owns the duty directly. The salon's own deployer duties (50(3) emotion/biometric; 50(4) deepfake including AI-simulated before/after) live 2026-08-02 with no grace; only the 50(2) marking duty has until 2026-12-02, and that is the provider's anyway.

---

## Version History

- **v1.4** (2026-07-20, skill-evaluator) — **Executed the fully-unblocked Artifact-4 review against the final Art. 50 guidelines (published 2026-07-20) and corrected a duty-ownership error the skill had carried since v1.1.** Four changes, all time-sensitive (applicability 2026-08-02): **(1)** Rewrote the Art. 50(1) framing across the State/EU gate block, Artifact 4, Design Principle 6, and the deployment notes — the chatbot-disclosure duty is a **provider** obligation binding the bot **vendor**, not the deployer-salon; the salon's operative task is to *confirm the vendor's bot discloses at first interaction and not disable it*, with a self-build/white-label carve-out that pulls the provider duty onto the salon. The earlier version called 50(1) "the primary operator duty," which described what the bot must do but misattributed *who the Act holds responsible* — the same failure class as the TX SB 140 error, so it is corrected explicitly rather than quietly. **(2)** Added the salon's *own* direct **deployer** duties — **50(3)** (emotion-recognition/biometric notice) and the **narrow 50(4)** labelling (deepfakes + public-interest text; human-reviewed text exempt) — and a **new med-spa 50(4) deepfake flag** for AI-simulated before/after imagery resembling a real person, which the deployer must label on first exposure and cannot discharge via the provider's 50(2) mark; added Artifact 4 deliverable 4 (a before/after deepfake label) cross-referenced to Artifact 3. **(3)** Corrected the grace-period scope: only the 50(2) marking duty gets the 2026-12-02 transition (and only for pre-existing systems); 50(1)/(3)/(4) are live 2026-08-02 with no grace. Added SME fine proportionality and the note that the 2026-07-22 signatory date is not an operator compliance deadline. Updated the worked example (Fresha bot = the provider; line 1 is what the clinic verifies) and citations from the May draft to the final guidelines. **(4)** Closed the **highest-priority carryover hand-off**: **Artifact 2 now carries a distinct, separately-checked AI-generated-voice consent grant** — artificial-voice marketing needs prior express written consent and the proposed federal standard would require naming AI specifically, so a generic "automated calls" line may not carry the load. This is the opt-in that `ai-receptionist-disclosure-and-escalation` treats as a hard go-live dependency for outbound voice; the spec, the worked-example checkbox, and the rationale all reflect it. Additive throughout except the corrected 50(1) attribution; no artifact or checklist item removed or renumbered.
- **v1.3** — Added a **"Quick Start — Which Artifacts Do I Need?"** decision tree at the top (two questions → a business-type × EU-exposure matrix telling the user which of the five deliverables to generate and which to skip, plus a note that state-gate detail need only be read for the states actually operated in). Addresses the skill's density ceiling: a salon with no EU exposure and no clinical services now reaches its 2–3 relevant artifacts without parsing all four gates. Additive; no prior content removed. Also corrected stale `last_eval_score` frontmatter (read 9.3; recorded 8.5 on 2026-06-29).
- **v1.2** — Refreshed the Artifact 4 Art. 50(1) chatbot-disclosure drafter note against the **2026-06-22** monitor hand-off: the Commission's Art. 50 **guidelines** were published in **DRAFT on 2026-05-08** (consultation closed 2026-06-03; final pending). Recorded the two draft sharpenings now safe to build to — disclosure at the **moment of first interaction** (not buried in ToS/settings/privacy policy) and extension of the duty to **agentic booking agents** that act on the client's behalf (book/reschedule/cancel), not only passive Q&A bots. Directional-but-not-final framing retained; re-check before 2026-08-02 for the authoritative text. Additive; no prior content removed.
- **v1.1** — Absorbed the regulatory gates handed off by the 2026-06-08 and 2026-06-15 landscape monitors. Added a Config Integration table (10 keys with per-key fallbacks) and a "State & EU Regulatory Gates" block covering **CA AB-489** (license-implication-language hard gate + standing-copy crawler-audit framing), **CA SB 351** (Good-Faith-Exam / Patient-Specific-Order consultation gate), and **CO SB 26-189** (AI-deployer duty, HIPAA-covered-vs-not, for AI hiring/financing tools, effective 2027-01-01). Rewrote **Artifact 4** against the **final EU Code of Practice (published 2026-06-10)**: leads with the Art. 50(1) chatbot-disclosure duty, scopes the Art. 50(4) labelling duty narrowly (deepfakes + public-interest text — no over-labelling of routine copy), notes the Art. 50(2) provider-side provenance marking, the optional Annex I icon as available-but-not-required, and the 2026-08-02 applicability / 2026-12-02 transitional cutoff. Added Review Checklist items 9 (AB-489) and 10 (SB 351 + EU chatbot disclosure) — appended, not renumbered, so existing cross-skill item references stay valid. Additive throughout; no prior content removed.
- **v1.0** — Initial four-artifact consent/disclosure/authorization pack + human-in-the-loop review checklist.
