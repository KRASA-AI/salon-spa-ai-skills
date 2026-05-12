# State-by-State Med Spa Regulation Reference — 2026

*Last updated: 2026-05-11. Compiled from landscape monitor runs 2026-04-12 through 2026-05-11. This is a curation reference for use by skills in this repo — not legal advice. Route to the practice's medical director and healthcare attorney for citation-level review and real-time status.*

---

## How This File Is Used

Skills in this repo reference this file for:
- State-specific refund-grace-window language (membership programs, pre-paid series)
- Supervision and ownership rules (who can own a med spa; what "medical director" requires)
- Cancellation policy hooks (deposit and refund language that varies by state)
- Compliance-checklist entries in `ai-consent-and-compliance-guardrails` and `cancellation-no-show-policy-author`

Skills should cite entries by state name and rule category only — never quote statutes verbatim. Route to counsel for the exact citation.

---

## Federal / Cross-State

### DEA Telehealth Prescribing Extension (COVID-era rules)
- **Status**: Extended through **December 31, 2026** (final extension per DEA; no further extensions expected).
- **Impact**: Med spas offering GLP-1, HRT, testosterone, or other controlled-substance prescribing via telehealth may continue under the COVID-era flexibilities through year-end 2026. Practices must plan for the transition to in-person-first prescribing beginning January 1, 2027.
- **Skill hook**: `sales/membership-program-builder` — any clinical-tier membership that includes a telehealth-prescribing component must flag the 2026 year-end deadline so the program is structured to survive the transition.

### HIPAA (ongoing)
- HIPAA violations can reach $50,000 per violation; annual fines can top $2 million. Cash-pay practices are not exempt.
- 2026 standard: encrypted electronic health records, limited user access controls, audit trails, regular risk assessments, multi-factor authentication.
- **Skill hook**: `ai-consent-and-compliance-guardrails`, `cancellation-no-show-policy-author` (PHI-aware SMS language), `membership-program-builder` (HIPAA-aware enrollment copy — no clinical service names in external SMS).

### EU AI Act Article 50 — AI-Generated Content Transparency
- **Applicability date**: **August 2, 2026**.
- **Status (as of May 2026)**: Second draft of the Code of Practice on Marking and Labelling of AI-Generated Content published; finalization expected early June 2026 (before the applicability date).
- **Impact**: Deployers of generative AI systems for professional purposes must clearly label AI-generated content in communications with the public. Affects any EU-client-facing booking confirmation, marketing email, or intake form that is AI-generated or AI-modified.
- **Skill hook**: `operations/ai-consent-and-compliance-guardrails` Artifact 4 (EU booking-confirmation notice). Schedule a v1.1 review of that artifact once the Code of Practice finalizes in early June 2026.

---

## State-by-State Entries

### Arizona
- **Status (2026)**: New bill introduced early 2026 (specific bill number not yet final per May 2026 monitor). Watch item.
- **Skill hook**: Flag in `ai-consent-and-compliance-guardrails` for med spas operating in AZ; re-check at June 2026 monitor cycle.

### California
- **AB-890 (effective 2026)**: Nurse practitioners with six or more years of experience, or who hold a Doctor of Nursing Practice (DNP) degree, may own and operate a med spa without physician supervision. Significant change in a state that has historically been strict on Corporate Practice of Medicine (CPOM).
- **Membership refund-grace window**: California Business & Professions Code §8599 and related provisions — health club and certain wellness service contracts require a **3–7 day right of rescission** depending on the contract type. Route to attorney for the exact applicable section for med spa memberships.
- **CPOM rule**: Physicians and now NPs (post-AB-890) may own the medical entity. Non-physician ownership structures require careful MSO/PC setup.
- **Skill hook**: `sales/membership-program-builder` (California refund-grace block); `cancellation-no-show-policy-author` (CA refund-grace for pre-paid series); `ai-consent-and-compliance-guardrails` (ownership structure compliance note).

### Colorado
- **HB 1024** (effective **August 5, 2025**): Med spas must prominently display their cancellation, refund, and membership terms in the digital booking flow and at the physical treatment location.
- **SB 130** (introduced February 2026, still in committee as of May 2026): Expanding medical-aesthetic ownership rules. Not yet enacted — watch for committee progress.
- **Skill hook**: `cancellation-no-show-policy-author` (CO prominent-display flag); `membership-program-builder` (CO compliance checklist item); `ai-consent-and-compliance-guardrails` (CO display rule for AI-generated notices).

### Florida
- **SB 1728 / HB 1429 — "The Medical Spa Prescription Drug Oversight Act"** (introduced January 2026, **effective July 1, 2026 if passed**):
  - Requires all medical spas to **register with the state** beginning July 1, 2026; failure to register results in disciplinary action, fines, suspension, or license revocation.
  - Requires designation of a "responsible person" — a licensed health care provider with physical presence sufficient to oversee prescription drug security and control.
  - Effectively requires a **separate pharmacy license** for med-spa practices that handle prescriptions.
- **Ownership**: Florida permits non-physician ownership of med spas. A licensed physician must serve as medical director.
- **Skill hook**: `cancellation-no-show-policy-author` (FL SB 1728 flag for the medical-director sign-off line); `membership-program-builder` (FL registration deadline); `ai-consent-and-compliance-guardrails` (FL pharmacy-license requirement note for clinical-tier offerings).

### Indiana
- **SB 282** (introduced 2026, **compliance deadline January 1, 2027**):
  - Requires mandatory registration of medical spas with the state's Medical Licensing Board.
  - Requires designation of a responsible practitioner.
  - Requires adverse-event reporting to the Medical Board no later than 5 days after a serious adverse event.
- **Skill hook**: `cancellation-no-show-policy-author` (IN registration deadline in the med-spa compliance footer); `membership-program-builder` (IN registration flag in the compliance checklist).

### Iowa
- **HSB 591 — Medical Spa Oversight Act** (introduced 2026):
  - Medical directors must be within **60 miles** of the delegated service location.
  - Medical directors must provide at least **4 hours of direct, on-site supervision per week**.
  - These are hard rules, not guidelines.
- **Skill hook**: `ai-consent-and-compliance-guardrails` (IA supervision rule for any med spa with a remote medical director arrangement); flag in `cancellation-no-show-policy-author` med-spa compliance footer if state = Iowa.

### Massachusetts
- **May 2025 Board of Registration of Cosmetology and Barbering** policy update: defines the boundary between cosmetic beautification (within cosmetology scope) and medical aesthetics (outside scope). Dermaplaning, chemical peels beyond certain depths, and similar procedures require medical oversight — cannot be performed by cosmetology licensees acting alone.
- **Skill hook**: `cancellation-no-show-policy-author` (MA cosmetology / med-spa scope clarification in the worked example and the things-to-verify checklist); `staff-training-guide` (scope-of-practice gate for MA practices).

### New Jersey
- **February 2026 Joint Rule — Board of Medical Examiners + Board of Pharmacy**: "Rent-a-doc" arrangements are prohibited. APNs and PAs must have written collaboration agreements that spell out delegated tasks, emergency plans, and supervision requirements. The medical director must be genuinely involved — a signature-only arrangement is a compliance risk.
- **Telehealth prescribing**: NJ now requires written collaboration agreements even for telehealth prescribing relationships.
- **Skill hook**: `cancellation-no-show-policy-author` (NJ joint-rule flag for med-spa compliance footer); `ai-consent-and-compliance-guardrails` (NJ collaboration-agreement note).

### New York
- **2026 enforcement wave**: NY state task force has run **223 inspections** and cited **87 clinics** for violations (fines, license suspensions, revocations). Primary violation categories: physician ownership requirements not met, out-of-scope-of-practice procedures, documentation failures.
- **Ownership rule**: Physician ownership required; no exceptions. Anything that breaks the skin is considered practicing medicine.
- **Skill hook**: `ai-consent-and-compliance-guardrails` (NY ownership and scope-of-practice hard gate); flag prominently in `staff-training-guide` scope-of-practice gate for NY practices.

### Ohio
- **2026 enforcement**: Ohio closed **30+ clinics** in 2026 for violations including improper sourcing, storage, documentation, supervision, and drug handling.
- **Requirement**: Documented physician-approved protocols and genuine physician oversight (not just a signed form).
- **Skill hook**: `ai-consent-and-compliance-guardrails` (OH enforcement climate note); `staff-training-guide` (OH documentation and physician-protocol requirements).

### Texas
- **Ownership**: Physicians only may own the medical entity. Medical director must be genuinely involved (not name-only).
- **Skill hook**: `ai-consent-and-compliance-guardrails` (TX physician-ownership hard gate).

### Washington
- **2026 status**: Washington Department of Health is **reviewing rules** for injectables, microneedling, IV hydration, and energy-based devices. Nothing final as of May 2026. Watch item.
- **Skill hook**: Flag in `ai-consent-and-compliance-guardrails` for WA med spas; re-check at June 2026 monitor cycle.

---

## Cross-Reference: Skills That Read This File

| Skill | What it reads from here |
|---|---|
| `operations/cancellation-no-show-policy-author` | State refund-grace windows; med-spa compliance footer state flags (CO, FL, IN, NJ, MA, IA) |
| `operations/ai-consent-and-compliance-guardrails` | EU AI Act Article 50; state-level HIPAA enforcement; ownership rules; NY / OH / TX / IA hard gates; Washington watch item |
| `sales/membership-program-builder` | State refund-grace windows (CA, CO, IN); FL registration deadline; DEA telehealth expiry; med-spa compliance checklist entries |
| `operations/staff-training-guide` | Scope-of-practice gate (MA, NY, OH); physician-protocol requirements (OH, TX); IA supervision hours |

---

*Refresh cadence: updated at each landscape monitor cycle when new state-level regulatory developments surface. Next scheduled check: June 2026 monitor cycle (EU AI Act Code of Practice finalization + CO SB 130 committee status + WA rule review update).*
