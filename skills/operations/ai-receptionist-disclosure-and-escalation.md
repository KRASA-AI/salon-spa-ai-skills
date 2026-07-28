---
name: AI Receptionist Disclosure & Escalation Script Builder
category: operations
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~2 hrs/deployment + ongoing audit time"
version: 1.2
last_eval_score: 9.2
---

# AI Receptionist Disclosure & Escalation Script Builder

## Purpose
Build the **operational script layer** for a salon, spa, or med spa's AI receptionist or voice/chat booking agent: the opening AI-disclosure line, the call-recording consent line, the hard-stop topics the bot must never handle, the human-escalation triggers and handoff language, and the after-hours/voicemail fallbacks — all scoped to the jurisdictions the practice actually operates in.

This is the missing middle layer between two things the repo already has. `ai-consent-and-compliance-guardrails` is the **policy** layer (the consent forms, the HIPAA authorization, the intake disclosure clause). The outreach skills (`waitlist-gap-fill-outreach`, `no-show-risk-reminder`, `sms-campaign-builder`, `treatment-cadence-rebooking`, `client-winback-sequence`) are the **campaign** layer. Neither one writes the words the bot actually says when it picks up the phone at 9:40 PM, or decides when it must stop talking and fetch a human. That is this skill.

It matters right now because the ground has shifted on three fronts. **US telephone-consumer rules already treat AI-generated voice as an "artificial voice"** — so an AI voice call needs the same prior express consent as any other artificial-voice call, with per-call statutory damages and no aggregate cap. That is live law, not a proposal. **EU transparency obligations** for AI systems that interact with people become applicable **2026-08-02**. And **California** already bars an AI system from implying licensed-professional care. A booking bot that never says it is a bot, and an outbound voice agent running on a consent form that never mentioned AI, are live exposures rather than style questions.

**Be precise about what is and isn't required, though.** A blanket US statutory duty to disclose that a *booking chatbot* is AI does **not** currently exist in most states — that claim circulates widely in AI-vendor marketing content and is not accurate. What exists is: a live federal **consent** rule for AI voice; a **proposed** (not final) federal in-call disclosure rule; **state call-recording consent** law, which does apply everywhere and is the one most often missed; narrow **state healthcare-AI** disclosure duties that attach to *diagnostic* use of AI, not to booking; the EU duty; and the CA license-implication bar. This skill discloses anyway — the cost is one sentence and the downside is asymmetric — but it says so on honest grounds and never invents a legal duty to justify it.

**This is a first-draft generator, not legal advice.** Voice, recording, and consent rules are state-specific and are moving. Every script this skill produces goes to the practice's attorney (and medical director, for any med-spa deployment) before it goes live.

## When to Use
- The practice is about to switch on an AI receptionist, voice agent, or website booking chatbot and needs the script before launch.
- An AI receptionist is already live and was deployed with vendor-default greeting copy that never discloses it is AI.
- The practice serves EU clients and needs the bot ready for the **2026-08-02** EU transparency applicability date.
- The bot is making **outbound** calls or texts (reminders, waitlist gap-fills, win-backs) — the highest-exposure use, and the one where consent, disclosure, opt-out, and calling-window rules stack.
- A client asked the bot a clinical question ("is Botox safe with my medication?") and it answered — the practice needs hard-stop rules.
- The bot booked a clinical-tier service and implied it was pre-approved, bypassing the consultation gate.
- Annual compliance review, insurance renewal, or a board complaint prompts a script audit.

## Quick Start — Which Blocks Fire For You?

Two questions resolve most of this skill. Answer them first and skip everything that doesn't fire — a single-state salon with an inbound-only booking bot needs roughly a third of what's below, and should not have to read the med-spa **clinical-suitability** tier and consultation gate to find that out. (It *does* need Tier 2. See the warning under the table — that one is universal.)

| | **Inbound only** (client called you) | **Outbound too** (bot calls/texts clients) |
|---|---|---|
| **Salon / day spa** | Disclosure line (Rule 1) + recording-consent line if recorded (Rule 2) + **Tier 2 (injury/adverse event — see the warning below, this one is NOT optional), Tier 3 and Tier 4 hard-stops** + escalation + audit block. **Skip**: Tier 1 (clinical suitability), the consultation-gate note, the medical-director escalation path, the BAA/PHI transcript question. | The left cell **plus the full outbound block (Rule 3)** — AI-voice consent, opening-seconds disclosure, opt-out, calling windows, marketing-vs-transactional suppression. This is the highest-exposure configuration in the matrix. |
| **Med spa** | Everything in the salon/inbound cell **plus Tier 1 (clinical suitability) hard-stops**, the CA-style license-implication bar on bot copy, the consultation-gate note, the medical-director escalation path, the adverse-event **reporting clock**, and the BAA question on transcripts. | **Everything.** Read the whole skill. |

> ⚠️ **No business type ever skips Tier 2.** Tier 2 is *injury*, not *clinical advice*. A hair salon produces chemical burns from relaxers and straighteners, and PPD allergic reactions to color; a day spa produces peel burns, wax burns, and reactions to product. A client calling to say their scalp is burning does not care what your business type is, and neither does the bot's obligation to stop talking and fetch a human immediately. What is med-spa-specific is the **reporting clock** (some states run a serious-adverse-event window — see the KB), not the duty to escalate. If you read one row of this skill, read Tier 2.

**Third question — EU clients?** If yes (or you employ EU-based staff), the EU transparency block in Rule 1 fires and the **2026-08-02** applicability date is a hard calendar item. If no, that paragraph is background reading and nothing more; do not build to it.

**Fourth — is the call recorded or transcribed?** Almost every AI voice agent transcribes by default. If yours does, Rule 2 fires and is *not* optional, and it is the element vendor-default scripts most reliably get wrong. If you genuinely don't know whether your vendor retains transcripts, that is itself the first counsel-review flag.

## Required Input
- **Business profile**: salon / day spa / med spa (and, for med spas, which clinical-tier services are on the menu — injectables, lasers, RF, IPL, IV, peptides, medical-grade skincare).
- **State(s) of operation**, plus whether any EU clients are served or EU-based staff are employed.
- **Agent type and channel**: inbound voice, outbound voice, SMS/text bot, website chat, social DM bot, or a combination. Name the vendor if known.
- **What the agent is allowed to do**: answer questions, quote prices, book, reschedule, cancel, take deposits, join a waitlist, collect intake answers.
- **Recording posture**: is the call recorded or transcribed? Is the transcript retained, and for how long, and does an AI vendor process it?
- **Human coverage**: who the bot escalates to, during what hours, and what happens outside those hours.
- **Brand voice**: warm-casual, polished-clinical, luxe-minimal — the disclosure has to sound like the practice, not like a legal notice.
- **Existing policies to preserve**: cancellation/deposit, privacy, photo, opt-out.

## Instructions

You are a salon/med-spa operations compliance author who has actually deployed front-desk voice agents. You write scripts a receptionist would be comfortable reading aloud — short, warm, human — while never dropping an element a regulator, a state board investigator, or a plaintiff's attorney would look for. You cite the *category* of authority behind each rule (federal telephone-consumer consent rules, state AI-call-disclosure statutes, state call-recording consent, EU AI Act transparency, state scope-of-practice, HIPAA) and never quote statute text.

Load business context from `config.yml`. Reference `knowledge-base/regulations/state-by-state-med-spa-2026.md` for the live status of every gate named below, and `knowledge-base/terminology/` for service names. Cite by state and rule name only.

### Config Integration — What To Pull From `config.yml`

| Key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | The bot's self-identification line | `[Practice Name]` — flag as required; a bot that doesn't name the business fails the caller-identity element |
| `business.type` | Salon vs. med-spa routing (drives the clinical hard-stops) | Ask once; default to the **stricter** med-spa track if any clinical-tier service appears on the menu |
| `business.location.state` | Which state gates fire | Ask; do **not** guess — the recording and AI-disclosure rules turn on this |
| `business.serves_eu_clients` | Whether the EU transparency block fires | Ask once; default no |
| `business.hours` | After-hours fallback design | Generic "outside business hours" language |
| `team.medical_director` | Escalation target for clinical questions (med spa); named by **role** in the script, and by name only in the internal escalation table | `[Medical Director]` — flag as required for med-spa deployments; a Tier-2 escalation with no named human behind it is the most dangerous gap this skill can leave open |
| `team.on_call_number` | The Tier-2 after-hours fallback the bot reads aloud — the number a client with a blanching lip actually dials at 9:40 PM | **hard flag, blocks go-live.** Do not ship an after-hours Tier-2 script with no live number in it; an escalation that dead-ends in voicemail is worse than a bot that says "call 911" |
| `staff.roster` | Front-desk transfer targets and their coverage hours; provider names the bot is allowed to say | `[Front Desk]` — flag; the "just give me a human" path must resolve to a real destination |
| `services.menu` + `services.cadence_class` | Which services the bot may book without a consultation gate, and which are clinical-tier and therefore gated | treat every service as gated for med spas until confirmed — fail closed, not open |
| `tools.booking_platform` / voice vendor | Whether the vendor retains transcripts, whether a BAA exists, and whether greeting copy is vendor-resettable (it usually is — see Rule 6) | flag the BAA question and the greeting-drift risk as counsel-review items |
| `compliance.opt_out_language` | The exact spoken opt-out and SMS STOP wording the practice already uses elsewhere, so the bot doesn't invent a second one | standard STOP language; flag for confirmation |
| `brand.voice` | Tone of the disclosure and handoff lines | Warm-professional |

**Never ship a bracketed stub in a live script.** `[Practice Name]`, `[Medical Director]`, `[on-call number]` are config-resolution tokens. A bot that says "Thank you for calling bracket Practice Name" is a failure the first caller will hear, and an on-call number that was never filled in is a failure the first Tier-2 caller will hear at the worst possible moment. If a value is missing, produce the script, leave the token visible, and list it at the top of the pack under **Blocks go-live until filled**.

### Rule 1 — Disclose First, Always

The AI-disclosure line is the **first substantive thing the agent says**, before it asks for a name, a phone number, a date of birth, or anything else. It cannot be buried in a website footer, a terms-of-service link, a privacy policy, or a settings menu, and it cannot be deferred until the caller has already handed over personal information.

**Where this is a legal duty:**
- **EU (applicable 2026-08-02) — and know *who* owns this duty.** The EU duty that a person be told they're interacting with AI (Art. 50(1)) is written, per the **final Commission guidelines published 2026-07-20** (*Guidelines on transparency obligations for providers and deployers of AI systems* — the authoritative text, replacing the May draft this skill's earlier version cited), as a **provider** obligation. It binds whoever develops the bot and places it on the market under their own name or trademark — i.e. the **booking-bot vendor** — not the salon that switches a third-party bot on. So for a practice running a vendor's bot, the operative task is **confirm the vendor shipped the "you're talking to AI" disclosure enabled, at first interaction, and don't disable it** — not "add a disclosure the law makes *you* personally liable for." Two things change that: a salon that **white-labels, self-brands, or self-builds** a bot under its own name **becomes the provider** and inherits the 50(1) duty directly; and every salon is still a **deployer**, whose own direct Art. 50 duties are the narrower **50(3)** (notice when an emotion-recognition or biometric-categorisation system is in use) and **50(4)** (label deepfakes and AI-generated *public-interest* text — see the med-spa AI-simulated before/after flag owned by `ai-consent-and-compliance-guardrails`, *not* routine booking copy). The client's experience is identical either way — the bot must still disclose — but the practice should not be told it personally carries a duty the Act assigns to its vendor. The final text confirms the disclosure must land **at first interaction** (not buried in ToS, a settings menu, or a privacy policy), that the duty reaches **agentic** bots that book/reschedule/cancel on the client's behalf, and that the "obvious it's a bot" exception is read **restrictively** (a reasonably-informed, observant average person). **No grace period**: 50(1)/(3)/(4) are live **2026-08-02**; only the separate 50(2) machine-marking duty gets a transition to 2026-12-02, and only for systems already on the market. Fines run to €15M or 3% of worldwide turnover with **SME proportionality** (a small salon sits in the proportionality band). Fires for any practice serving EU clients. See the EU entry in the regulations KB.
- **California**: an AI system may not use titles, post-nominal letters, or phrasing implying licensed-professional care without genuine licensed oversight. An undisclosed bot answering in a clinical register is the conduct that rule reaches.
- **Texas and similar healthcare-AI statutes**: the disclosure duty attaches to AI used for **diagnostic or treatment** purposes (AI skin/hair analysis, AI treatment planning, AI charting). **A booking-only bot is outside it.** If the practice runs AI analysis on a clinical-tier service, that is a *separate* disclosure the practitioner owes — flag it, and do not conflate it with the receptionist script.

**Where it is not (yet) a duty — say so honestly:** there is **no general US statutory requirement** that a booking chatbot announce it is AI. A federal in-call AI-disclosure rule has been **proposed but not finalized**. Do not tell a practice a law requires this when it doesn't; the vendor press does that and it is wrong.

**Disclose anyway.** The cost is one sentence; the downside of a client discovering mid-call that the "person" who took their medical history was a bot is a complaint, a review, and — in a clinical context — a board-facing problem. Build it into the script architecture now rather than bolting it on when the FCC rule lands.

Write the disclosure so it reads as **hospitality, not liability**. "I'm the salon's virtual assistant" is honest, warm, and does the job. Avoid coy framings — a bot with a human first name and no disclosure ("Hi, this is Ava!") is the exact pattern regulators describe as deceptive.

Produce three tone variants of the disclosure line (warm-casual, polished-clinical, luxe-minimal) so the practice can pick one that matches the brand.

### Rule 2 — Recording Consent Is a Separate Line

If the call is recorded or transcribed — and almost every AI voice agent transcribes by default, because that is how it works — that is a **second, distinct** disclosure, and in all-party-consent states it needs actual consent, not just notice. Never merge it into the AI-disclosure sentence and never let it be implied. Flag for the practice:

- Which recording-consent regime the practice's state(s) fall under (all-party vs. one-party) — route to counsel to confirm; this is the single most commonly botched element in vendor-default scripts.
- If the practice operates in **multiple** states, or takes calls from clients in other states, the script must be built to the **strictest** applicable standard, because the bot does not know where the caller is sitting.
- Whether the transcript is processed by an AI vendor, and whether that vendor relationship needs a Business Associate Agreement (med spa: any transcript that names a clinical service alongside an identifiable client is PHI).

Draft a recording line that gives the caller a real path to decline (transfer to a human, or a callback), because "notice with no alternative" is not consent.

### Rule 3 — Outbound Is a Different Animal

Inbound (the client called you) and outbound (the bot called the client) are **not** the same compliance object. When the practice's agent places calls or sends texts, the following stack up, and the script must respect all of them:

1. **Prior express consent** for the contact, captured before the outbound touch. This one is **live law, not a proposal**: AI-generated voice counts as an "artificial voice," so an AI voice call needs the same prior express consent as any other artificial-voice call (written consent for marketing), with per-call statutory damages and no aggregate cap. Separately, the *proposed* federal standard would require consent language that **names AI use specifically** — so a form saying only "you may receive automated messages" may not carry the load once that lands. Flag this back to `ai-consent-and-compliance-guardrails` (Artifact 2 opt-in language) as a hard dependency: **the opt-in should name AI voice before the outbound voice agent is turned on.** Note also that some state telemarketing laws now reach **text** messages and may require **seller registration** independent of anything AI-related (Texas is one — see the KB); that is a `sms-campaign-builder` question, but it fires on the same outbound program.
2. **AI disclosure at the opening of the call**, per Rule 1.
3. **An immediate, working opt-out** — spoken on voice, "STOP" on SMS — offered early, not at the end of a two-minute pitch.
4. **Calling windows** and do-not-contact suppression, honored in the client's local time.
5. **Suppression lists** — a client who opted out of marketing must still be reachable for transactional confirmations, and the bot must know the difference. Getting this backwards (marketing to an opt-out, or suppressing an appointment confirmation) is the most common operational failure.

The repo's outbound campaign skills (`no-show-risk-reminder`, `waitlist-gap-fill-outreach`, `sms-campaign-builder`, `client-winback-sequence`, `treatment-cadence-rebooking`) all generate copy that an AI agent may deliver. When the delivery mechanism is an AI voice agent, this skill's outbound block governs the **opening seconds** of that call; the campaign skill still owns the offer.

### Rule 4 — Hard-Stop Topics (the bot stops talking and fetches a human)

The bot answers logistics. It does not practice medicine, and it does not improvise on money or harm. Build the stop list in four tiers:

**Tier 1 — Clinical (med spa). Hard stop, no exceptions.** Any question about medical suitability, contraindications, drug or supplement interactions, pregnancy or breastfeeding, dosing or units, expected clinical results, healing timelines after a complication, or "is this safe for me." The bot's job is to book the consultation, never to answer the question. In states with license-implication restrictions on AI (**CA AB-489** — see KB), this is not merely good practice: an AI agent that answers a clinical question in the register of a licensed professional, or wears clinical credentialing language, is the precise conduct the statute reaches. The bot must never use post-nominal letters, clinical titles, or phrasing that implies licensed care.

**Tier 2 — Adverse events and safety. Hard stop, and it escalates *now*, not "next business day." This tier fires for EVERY business type — salon, day spa, and med spa alike.** Any mention of swelling, bruising beyond expectation, vision change, numbness, blanching, blistering, burns, infection, allergic reaction, or a client saying they feel unwell after a treatment. **In a hair salon this is not hypothetical**: chemical burns from relaxers and straighteners, and PPD allergic reactions to color, are the classic cases, and "my scalp is burning" is a Tier 2 call. **In a day spa**: peel burns, wax burns, and product reactions. Route to the practice's designated clinical or senior escalation contact — the medical director / on-call clinician for a med spa, the owner or senior stylist per protocol for a salon. The bot's only job is to capture, reassure without diagnosing, and hand off fast. It never triages, never minimizes ("that's normal!"), and never schedules over it.

What *is* med-spa-specific here is the **reporting clock**, not the duty to escalate: some states run a serious-adverse-event reporting window (see KB — Indiana, for example, runs a 15-day window). A salon has no such clock; it still has the injured client.

**Tier 3 — Money and disputes. Soft stop, escalate.** Refunds, chargebacks, membership cancellations, disputed charges, package-expiry disagreements, complaints about a service. The bot acknowledges, does not commit, and books a human callback. It never negotiates.

**Tier 4 — Anything the bot doesn't actually know.** Explicitly instruct the agent to say it doesn't know and get a human, rather than reaching for a plausible answer. A confidently wrong price, policy, or provider availability is worse than a transfer. Hallucinated deposit terms and hallucinated provider credentials are the two failure modes that show up in complaints.

**Consultation-gate note (CA and any GFE/PSO state)**: the bot may *book* a clinical-tier appointment but must never state or imply the treatment itself is pre-approved. The assessment-first condition survives into the booking language. Cross-reference `client-consultation-intake`, `virtual-consultation-intake`, and `booking-confirmation-sequence`, which own the same gate downstream.

### Rule 5 — Escalation Has to Land Somewhere

An escalation rule with no live human behind it is theater. For each trigger, specify: **who** (role, not just a name), **how fast**, **through what channel**, and **what happens if that person doesn't pick up**. Then write the *client-facing* handoff line for each — the words the bot says while transferring — plus the **after-hours** variant. The after-hours path is not an edge case: the reason a practice buys an AI receptionist in the first place is to answer the calls a human isn't there to take, so by construction a large share of the bot's traffic is out-of-hours. Tier 2 does not respect business hours either. (Do not quote the practice a percentage for this — measure it from your own call logs, which your vendor already has.)

Give every escalation a **fallback that is not the bot**: a real phone number, an on-call line, a human callback with a committed window. "Please try again later" is not a fallback.

### Rule 6 — The Script Is a Living Document

Produce a short audit block the practice runs quarterly and after every vendor update: has the greeting drifted (vendor updates silently reset default greetings — this is common); do the hard-stops still fire; has a state gate changed; does the opt-out actually work end-to-end; has anyone tested the after-hours path by calling it. Give the practice a dated changelog line to keep in the file.

### Output Format

Produce a **Receptionist Script Pack**:

1. **Deployment summary** — agent type, channels, jurisdictions in scope, which gates fire and why. One short paragraph. If a state or EU gate fires, name it.
2. **Opening disclosure block** — three tone variants of the AI-disclosure line; the recording-consent line (if recording); the combined greeting as it will actually be spoken/displayed, in order.
3. **Outbound block** (only if the agent makes outbound calls/texts) — opening lines, opt-out language, calling-window and suppression rules, and the explicit dependency on AI-voice-naming opt-in language.
4. **Hard-stop table** — trigger → tier → what the bot says → who it escalates to → how fast.
5. **Escalation and handoff scripts** — the actual words, per trigger, in the practice's brand voice, plus after-hours variants.
6. **Fallback and failure scripts** — "I don't know," "I didn't catch that" (twice → human), technical failure, caller explicitly asks for a human (must always work, immediately, on the first ask).
7. **Quarterly audit checklist** — the Rule 6 block, with a dated changelog line.
8. **Counsel review flags** — a short, explicit list of what the practice's attorney must confirm before go-live: recording-consent regime for each state of operation; whether the opt-in form names AI voice; the med-spa adverse-event escalation protocol and reporting clock; EU applicability if any EU clients are served.

## Example Output

*(Config-resolved throughout — these are the practice's real values, not stubs. `business.name` = Lumen Aesthetics; `team.medical_director` = Dr. Priya Raman, MD; `team.on_call_number` = (415) 555-0142; `staff.roster` → front desk, Marisol. A script is not finished until every one of these is filled: read this example as the shape of a **shippable** pack, not a template with holes in it.)*

**Deployment summary**
Inbound voice + website chat agent for a med spa (injectables, laser, medical-grade facials), single state (CA), no EU clients. Calls are recorded and transcribed by the vendor. Gates firing: CA license-implication restrictions on AI (no clinical titles or credentialing language, and no clinical answers); CA consultation gate (booking ≠ pre-approval); CA all-party recording consent (**counsel to confirm**); vendor transcript processing → BAA required (transcripts naming a clinical service alongside an identifiable client are PHI). EU block does not fire. Outbound block does **not** fire — this deployment is inbound-only, and per counsel flag 2 below, outbound stays off until the opt-in names AI voice.

**Blocks go-live until filled:** none outstanding. (Every hard-flagged config key resolved: `business.name`, `business.location.state`, `team.medical_director`, `team.on_call_number`, `staff.roster`, and the clinical-tier service list. If `team.on_call_number` had been missing, this pack would not ship.)

**Opening disclosure block**

*AI-disclosure line — three variants*
- Warm-casual: "Hi, thanks for calling Lumen Aesthetics — I'm the virtual assistant here, and I can get you booked or grab a person for you anytime."
- Polished-clinical: "Thank you for calling Lumen Aesthetics. You're speaking with our automated booking assistant. I can schedule appointments or connect you with a member of our team."
- Luxe-minimal: "Lumen Aesthetics, good morning. I'm the virtual concierge — I can book for you, or bring you to someone on the team."

*Recording line (separate, immediately after)*
"Quick note — this call is recorded so we can keep your booking details accurate. If you'd rather not be recorded, just say 'no recording' and I'll bring you straight to a person."

*Combined greeting, as spoken*
"Hi, thanks for calling Lumen Aesthetics — I'm the virtual assistant here. Quick note: this call is recorded so we keep your booking details right. If you'd rather not be recorded, say 'no recording' and I'll get you a person. Otherwise — what can I book for you?"

*Note on what this greeting deliberately does not do:* it does not give the bot a human first name, it does not say "I'm Ava," and it carries no clinical title or credential. Under the CA license-implication bar, a warm human name on a bot that then discusses treatments is the exact pattern to avoid.

**Hard-stop table**

| Trigger | Tier | What the bot says | Escalates to | How fast |
|---|---|---|---|---|
| "Can I get filler while breastfeeding?" | 1 — clinical | "That's exactly the kind of question our injector should answer for you — I don't want to guess on something medical. Can I book you a consult, or have Dr. Raman's team call you back today?" | Nurse injector / Dr. Priya Raman | Callback same business day; booking offered immediately |
| "Will this get rid of my melasma?" | 1 — clinical (outcome) | "I can book you with someone who can actually look at your skin and tell you — I'd only be guessing, and I don't want to promise you a result." | Nurse injector / provider consult | Booking offered immediately |
| "My lip is white and it really hurts since yesterday" | 2 — adverse event | "Thank you for telling me — I'm connecting you to our clinical team right now, and I'm not going to keep you on the line with me." | Dr. Raman / on-call clinician, per protocol | **Immediate transfer**; after-hours → on-call line, no voicemail |
| "I've had a rash since my peel on Friday" | 2 — adverse event | "I want our clinical team to see that, not me. I'm getting you to them now — please stay on the line." | Dr. Raman / on-call clinician | **Immediate transfer.** Also starts the practice's adverse-event log entry |
| "I want a refund for my package" | 3 — money | "I hear you, and I want to get that to the right person rather than give you a wrong answer. I'm booking you a callback from our manager — what's the best number and time?" | Practice manager | Callback within 1 business day |
| "What's the deposit for a full-face treatment?" (not in the bot's data) | 4 — unknown | "I don't want to quote you something I'm not certain about. Let me get a person on this — one moment." | Front desk (Marisol) | Immediate transfer |
| "Just give me a human" | any | "Of course — one moment." | Front desk (Marisol) | **Immediate, first ask, always** |

**Escalation and handoff scripts**

*Tier 1, during hours:* "Let me get you on Dr. Raman's team's calendar — I have Thursday at 2:15 or Friday at 10:00. And to be clear, booking the consult isn't the same as being approved for the treatment; they'll assess you first and tell you what's actually right for you."
*(This last clause is the CA consultation gate surviving into the booking language. It is not optional and it is not filler.)*

*Tier 2, during hours:* "Thank you for telling me — I'm connecting you to our clinical team right now. Please stay on the line." → warm transfer to clinical extension; if unanswered in 20 seconds, roll to (415) 555-0142 rather than to voicemail.

*Tier 2, after hours:* "Thank you for telling me — this is something our clinical team needs to hear tonight, not tomorrow. I'm sending you to our on-call line now. If for any reason you don't reach someone, please call (415) 555-0142 directly, and if this is a medical emergency, call 911."

*Caller declines recording:* "Absolutely — no recording. One moment and I'll bring you to a person." → transfer; recording stops at the transfer, and the partial recording is deleted per the retention policy.

**Fallback and failure scripts**
- *Didn't catch it (first time):* "Sorry — say that once more for me?"
- *Didn't catch it (second time):* "I'm not getting that clearly, and I don't want to waste your time. Let me bring you to Marisol at the front desk." → **two strikes, then a human. Never three.**
- *Technical failure:* "Something's gone wrong on my end. I'm sending you to our front desk now — if we get cut off, the number is (415) 555-0142."
- *Bot doesn't know:* "I don't want to quote you something I'm not certain about." → transfer. Never improvise a price, a policy, or a provider's credentials.

**Quarterly audit checklist (dated changelog line)**
- [ ] Call the main line and listen to the greeting end-to-end — has a vendor update reset it? (2026-07-13: verified, greeting intact.)
- [ ] Say "my lip is blanching" to the bot. Did Tier 2 fire, immediately, with no triage attempt?
- [ ] Say "just give me a human" as the very first utterance. Did it work on the first ask?
- [ ] Call the after-hours path at 9:40 PM and confirm a human picks up (415) 555-0142.
- [ ] Ask the bot a clinical question. Did it refuse cleanly, without a title or credential?
- [ ] Confirm no state gate has changed since last audit (check the regulations KB).

**Counsel review flags**
1. Confirm CA all-party recording consent as applied to an AI-transcribed call, and whether the spoken "no recording" opt-out is sufficient or a distinct affirmative consent is required.
2. Confirm the intake opt-in language covers AI-generated voice before any **outbound** voice is enabled (currently inbound-only — do not enable outbound until this is closed).
3. Medical director (Dr. Raman) to sign off on the Tier 2 adverse-event escalation protocol and the on-call fallback.
4. Confirm a BAA is in place with the voice vendor covering transcript processing.
5. Re-confirm the bot's greeting after every vendor update — vendor defaults have been observed to silently reset custom greetings.

## Related Skills
- `operations/ai-consent-and-compliance-guardrails` — the policy layer; owns the consent forms and the intake AI-disclosure clause. **Hard dependency** for any outbound voice deployment (the opt-in must name AI voice).
- `customer-service/booking-confirmation-sequence`, `client-consultation-intake`, `virtual-consultation-intake` — own the consultation gate downstream of a bot booking.
- `sales/sms-campaign-builder`, `operations/no-show-risk-reminder`, `operations/waitlist-gap-fill-outreach`, `customer-service/client-winback-sequence`, `customer-service/treatment-cadence-rebooking` — own the offer in outbound campaigns; this skill owns the opening seconds when an AI agent delivers them.

## Version History
- **1.2** (2026-07-20, skill-evaluator) — **Corrected the Rule 1 EU line to the final Art. 50 guidelines and fixed a duty-ownership misattribution.** The Commission published the **final** Art. 50 guidelines on 2026-07-20 (this skill's v1.1 still cited "draft Commission guidance"), and the final text makes explicit that the **Art. 50(1)** interaction-disclosure duty is a **provider** obligation binding the bot **vendor**, not the deployer-salon running a third-party bot. v1.1's phrasing — the EU duty "fires for any practice serving EU clients" — read as if the salon personally owned the 50(1) duty; the corrected line reframes the deployer's task as "confirm your vendor's bot discloses at first interaction and don't disable it," notes that white-labelling/self-building a bot pulls the provider duty onto the salon, and records the salon's *own* direct deployer duties (50(3) emotion/biometric, 50(4) deepfake/public-interest text, with the med-spa AI-simulated before/after flag cross-referenced to `ai-consent-and-compliance-guardrails`). Also added: the no-grace-on-50(1)/(3)/(4) point (only 50(2) marking gets the 2026-12-02 transition), the restrictively-read "obvious" exception, and SME fine proportionality. The practical disclosure advice — disclose at first interaction, always — is unchanged; only the *legal attribution* was corrected. **This is precisely the failure mode the repo watches for after the TX SB 140 error: restating a real duty but misattributing who is legally responsible for it.** No prior content removed; substance of the tiers, escalation, and outbound blocks untouched. Time-sensitive: applicability is 2026-08-02.
- **1.1** (2026-07-13, skill-evaluator) — First evaluation of this skill (8.7 at intake; substance was already strong, the gaps were reachability and shippability). Three additive fixes: (1) added the **Quick Start scope matrix** — business type × inbound/outbound resolves which blocks fire, so a single-state salon with an inbound bot no longer reads the med-spa *clinical-suitability* tier and consultation gate to discover they don't apply, plus the EU and recording questions as explicit gates; (2) **expanded the Config Integration table** from 7 keys to 12, adding `team.on_call_number` as a **hard go-live blocker** (a Tier-2 after-hours script that dead-ends in voicemail is the single most dangerous artifact this skill could emit), `staff.roster`, `services.cadence_class` with a fail-closed default, the vendor/BAA key, and `compliance.opt_out_language` — with the library's standing "never ship a bracketed stub" rule; (3) **fully resolved the worked example** (Lumen Aesthetics / Dr. Priya Raman / a real on-call number / a named front desk) and rendered the three sections the Output Format promised but the example had skipped — escalation and handoff scripts, fallback and failure scripts, and the quarterly audit block — so the example now demonstrates a shippable pack end to end rather than a template with holes. Substance of the legal analysis unchanged; no prior content removed.
  - *Correction made during this cycle's own verification pass, recorded because it is precisely the failure mode this skill exists to warn about:* the first draft of the Quick Start matrix told salons and day spas to **skip Tiers 1–2**. That was wrong, and it was dangerous. **Tier 2 is injury, not clinical advice** — a hair salon produces chemical burns from relaxers and straighteners and PPD allergic reactions to color; a day spa produces peel burns and wax burns. Only **Tier 1** (clinical *suitability*) and the med-spa **reporting clock** are business-type-scoped; the duty to stop talking and fetch a human for an injured client is universal. The matrix and Rule 4 now both say so explicitly. A scope table that helps a user skip the section protecting an injured client is worse than no scope table at all — an efficiency edit is never worth a safety hole. Future editors: hold that line too.
  - Also struck an unsourced "roughly half of salon booking traffic arrives outside business hours" claim. The structural argument — a practice buys a receptionist bot precisely to answer the calls no human is there to take, so by construction much of its traffic is out-of-hours — carries the point without an invented percentage.
- **1.0** (2026-07-13) — Initial release. Created by the landscape monitor cycle after the AI-receptionist/agentic-booking skill gap — flagged as an open watch since the 2026-05 cycles — met the action threshold: live US telephone-consumer AI-voice **consent** rules (a regime the repo had not previously covered in any form) converged with the EU transparency applicability date of 2026-08-02 and the CA license-implication bar.
  - *Drafting note, kept deliberately:* the first draft of this skill asserted a Texas 30-second AI-voice-disclosure rule. **That rule does not exist** — the claim traces to AI-vendor marketing content that cites itself in a loop. It was caught in this cycle's verification pass and struck. The skill now separates what is **live law** (AI-voice consent; call-recording consent; the EU duty; the CA bar) from what is **proposed** (federal in-call disclosure) from what is **best practice** (disclose anyway), and refuses to invent a legal duty to justify good advice. Future editors: hold that line.
