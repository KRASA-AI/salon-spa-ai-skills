---
name: AI Consent & Compliance Guardrails for Client Communications
category: operations
tools: [claude, chatgpt]
difficulty: advanced
time_saved: "~90 min/policy update"
version: 1.0
last_eval_score: null
---

# AI Consent & Compliance Guardrails for Client Communications

## Purpose
Generate the four consent, disclosure, and authorization artifacts that a salon, spa, or med spa needs when AI is drafting, routing, or analyzing client communications: (1) an AI-disclosure clause for intake forms, (2) a HIPAA-compliant marketing authorization separating clinical records from marketing outreach, (3) a before/after photo authorization scoped per platform and revocable, and (4) a short booking-confirmation notice that meets EU AI Act Article 50 transparency for any salon that has EU clients. Also produce a human-in-the-loop review checklist that defines exactly which AI drafts need a licensed provider's sign-off before they go out.

This skill operates at the policy layer — it is the guardrail that the downstream message skills (`review-response-writer`, `treatment-cadence-rebooking`, `sms-campaign-builder`, `social-caption-writer`, `social-content-calendar-planner`, `retail-product-recommender`) all draw from. It is not a replacement for licensed legal counsel; it is a first-draft generator that a salon's attorney and medical director can then mark up.

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

Load business context from `config.yml`. Reference `knowledge-base/terminology/` for service and product names, and `knowledge-base/policies/` for any existing policy snippets to preserve.

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

For non-medical salons: produce a simpler opt-in that still meets TCPA (for SMS) and GDPR (for EU clients) — explicit affirmative consent, clear purpose, right to withdraw.

**Artifact 3 — Before/After Photo Authorization (per platform, revocable)**

A separate form for any image that will appear on social or marketing channels. Must:
- Name the specific platforms (Instagram feed, Reels, Stories, TikTok, Facebook, Pinterest, website gallery, email nurture, case-study deck). Client can grant or withhold per platform.
- State whether the face is cropped / blurred / fully visible.
- Disclose any AI editing (color correction, lighting normalization, retouching) and require a second checkbox if retouching beyond white-balance is intended.
- State the authorization duration and a one-line revocation path ("reply or email CONSENT-REVOKE to [address] and we will remove within 30 days from all channels we control").
- Include the FTC endorsement disclosure if the client is being compensated with services, discounts, or product in exchange for the image.

**Artifact 4 — EU AI Act Booking-Confirmation Notice**

A single sentence to append to the booking confirmation (Touch 1 in the `booking-confirmation-sequence` skill) when the booking was processed by a conversational AI, an AI smart-scheduler, or an AI receptionist. Also produce a slightly longer privacy-page paragraph the booking confirmation links to. Satisfies Article 50 transparency for EU clients; harmless and increasingly expected for US clients too.

### Human-in-the-Loop Review Checklist

A one-page gate the practice staples to the AI-drafting workflow. Every outbound AI draft is checked against it before it can be approved to send. The licensed provider or a designated, trained staff member signs off. Items to include:

- Does the draft contain any clinical claim, treatment-outcome promise, or before/after percentage that is not in the practice's pre-approved claim library?
- Does the draft name a prescription product (Botox, Dysport, Daxxify, Restylane, Juvederm, etc.) in a promotional way that could trigger manufacturer compliance flags?
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
6. **Mirror the EU AI Act timeline.** Article 50 transparency deadlines are fixed calendar events; build toward them now rather than during audit season.

### Output Format

Return all five deliverables in this order, each in its own clearly labeled section:

1. **Artifact 1 — AI Disclosure Clause** (120–180 words, ready to paste into the intake form).
2. **Artifact 2 — Marketing Authorization** (full form with signature, date, channel checkboxes, expiration, revocation method).
3. **Artifact 3 — Photo Authorization** (full form with per-platform grants, retouch disclosure, revocation path, FTC disclosure if applicable).
4. **Artifact 4 — EU AI Act Booking Notice** (one-sentence confirmation append + linked privacy paragraph).
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

[Clinic Name] may use the following categories of my information to personalize what I receive: appointment history, service category received, product purchases, and stated preferences. [Clinic Name] will not use my specific diagnosis, medication list, or clinical note content for marketing.

This authorization expires 24 months from the date below. I can revoke it at any time by replying STOP (SMS), clicking "unsubscribe" (email), or emailing privacy@[clinicdomain]. Revocation takes effect within 10 business days.

Signature: __________________________ Date: __________

*Rationale:* Meets HIPAA marketing-authorization requirements (separate form, enumerated uses, non-conditioned, revocable, expiring); layers TCPA per-channel express written consent for SMS; provides GDPR-compatible lawful basis for EU clients. Does not cover employee/vendor data. Attorney focus: confirm 24-month expiration aligns with the practice's internal re-consent cadence and with California's evolving data-broker rules.

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

**Artifact 4 — EU AI Act Booking Notice**

*Append to booking confirmation (Touch 1):*
"This booking was processed with help from an AI scheduling assistant. A member of our team reviews every appointment before it's finalized. More on how we use AI: [clinicdomain]/privacy#ai."

*Privacy-page paragraph:*
"We use an AI scheduling assistant to help answer inquiries, suggest appointment times, and draft confirmation messages. The assistant accesses your name, contact details, service interest, and appointment history. It does not make treatment decisions. A trained team member reviews its drafts before they reach you. You can opt out of AI-assisted scheduling by calling or emailing us, and we will match you with a human-only path. If you are based in the EU, the assistant meets the transparency requirements of the EU AI Act as of August 2026."

*Rationale:* Satisfies EU AI Act Article 50 transparency obligations for systems interacting with natural persons; is also compatible with emerging US state-level AI disclosure rules. Does not constitute a full AI System Impact Assessment — that sits with the AI provider (Fresha, TrueLark, etc.) under the AI Act's deployer-vs-provider split. Attorney focus: confirm whether the practice's EU client base triggers full deployer obligations under Article 26 rather than just Article 50 transparency.

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
