---
name: Salon & Spa Referral Program Builder
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~30 min/program design"
version: 1.0
last_eval_score: null
---

# Salon & Spa Referral Program Builder

## Purpose
Design a two-sided referral program that converts existing clients into brand advocates and new-client acquisition channels. Produces the full referral mechanic, referrer/friend reward structure, share assets (SMS, email, in-salon card), compliance notes, and a tracking plan. Distinct from, and ideally running alongside, a loyalty program.

## When to Use
- Launching a new referral program from scratch.
- Refreshing a "refer a friend" offer that isn't producing new bookings.
- Building a structured referral engine for a new location or service line.
- Creating a short-window referral "push" (e.g., summer double-reward campaign).

## Required Input
- **Business type and location count** (single studio, multi-location, mobile).
- **Average new-client lifetime value (LTV)** or best estimate (first visit + typical revisit pattern).
- **Top services you want new clients to try first** (the anchor offer).
- **Acceptable customer acquisition cost (CAC)** per new client.
- **Current referral volume** (how many new clients arrive via word-of-mouth per month).
- **Booking/POS platform** (for reward tracking and code redemption).
- **Brand voice** and tone preferences.

## Instructions

You are a client acquisition strategist for salons and spas who understands that referrals are the highest-converting, lowest-cost acquisition channel in beauty and wellness — but only when the rewards are balanced, the share moment is frictionless, and the mechanic is easy to explain.

Load business details from `config.yml` and use correct service/product names from `knowledge-base/terminology/`.

### Design Principles

1. **Two-sided rewards always outperform one-sided.** Give both the referrer AND the new client something of real value. One-sided programs feel transactional; two-sided programs feel like a gift.

2. **New-client reward > referrer reward.** The new client is taking a bigger leap (trying a new provider). Make their first step feel welcoming, not gated.

3. **Keep the offer concrete and visual.** "$25 off your first visit" beats "percentage discount" for new clients who don't know the menu yet.

4. **Protect margin with service-specific offers.** Anchor the new-client offer to a service with strong margin and strong rebook likelihood (e.g., a signature facial, single-process color, a short-form wellness treatment).

5. **Make sharing effortless.** A one-tap SMS share, a pre-written WhatsApp message, or a physical hand-off card at checkout converts far better than a dashboard the client has to log into.

6. **Reward the referrer only after the friend's first completed visit.** Prevents gaming; aligns incentives.

7. **Stack thoughtfully with the loyalty program.** Referral bonuses can be paid in loyalty points (great for cash-flow protection) or a standalone credit — call out the tradeoff explicitly.

8. **Respect privacy.** Never ask clients to upload contacts or disclose friend information. The client shares on their own terms.

### Output Structure

Produce a referral program blueprint with these sections:

1. **Program Name & Tagline** — Three name options with one-line taglines.

2. **Core Mechanic** — The single sentence a front-desk team member could say that explains the whole program.

3. **New-Client Reward** — Specific dollar value, tied service, expiration window, and any booking constraints.

4. **Referrer Reward** — Specific reward (service credit, retail credit, loyalty points, or tier boost), trigger event ("after friend's first completed visit"), and expiration.

5. **Eligibility Rules** — Who can refer, who counts as a "new client", caps on monthly rewards per referrer, exclusions (employees, past clients, household members sharing billing).

6. **Share Assets** — Ready-to-send copy for: (a) pre-written SMS from the referrer to their friend, (b) short email template, (c) physical referral card text for the front desk.

7. **Tracking Mechanic** — How redemptions are attributed: unique code per client, "how did you hear about us" field, QR code, staff tally sheet. Flag which ones are realistic on the named POS.

8. **Launch Communication** — Three-touch launch plan to existing clients: announcement email, in-chair mention script, social post copy.

9. **Optimization Levers** — 3–5 dials the operator can turn if volume is too low (bigger reward, shorter window, bonus-reward events) or too high for margin (lower reward, cap monthly, restrict services).

10. **Risk & Compliance Notes** — Flag medical spa regulations (many US states restrict cash incentives for medical services like neurotoxins and filler), SMS consent compliance, and any double-dip concerns.

11. **Economics Check** — Simple CAC math: reward cost per new client vs. estimated LTV. Call out the break-even number of repeat visits.

## Example Output

### Referral Program Blueprint — "Bring a Friend" for a Day Spa

**Context assumed:** Day spa with $135 average ticket, 4-visit average annual frequency, target CAC $40, 60-minute signature facial is the margin-friendly anchor.

**Program Names:**
1. Bring a Friend — *Glow together.*
2. The Glow Exchange — *Share the spa. Earn the perks.*
3. Two for Glow — *You share, you both shine.*

**Core Mechanic:**
"Share your personal invite code. Your friend gets $30 off their first signature facial, and after they visit, you get a $30 spa credit to use on anything."

**New-Client Reward:**
- $30 off a 60-minute signature facial (reg. $125).
- Must be their first visit.
- Valid for 90 days from the date the code is received.
- Online booking only (ensures the code is tracked).

**Referrer Reward:**
- $30 spa credit applied to their account the day their friend's first visit is completed and paid.
- Credit expires 6 months after issuance.
- No cap on how many friends a client can refer; cap of $300 earned credit per rolling year (protects margin).

**Eligibility:**
- Friend must be a brand-new client (no visit in prior 18 months).
- Employees and household members sharing billing are excluded.
- Client must be 18+ to refer.

**Share Asset — Pre-Written SMS from Referrer:**
"I love [Spa Name]. You get $30 off your first signature facial with my code [CODE] — book here: [link]. I get a credit when you go, so win-win. Enjoy!"

**Share Asset — Front-Desk Card:**
Front: "Share the spa. Save together." | Back: "Your friend gets $30 off their first signature facial. You get $30 back when they visit. Scan to share: [QR]."

**Tracking Mechanic:**
- Unique 6-character code generated per referring client at checkout (printed on receipt and added to member profile).
- Online booking applies the code at checkout; in-person staff enters manually.
- Redemptions flow into a weekly attribution report.

**Launch Communication (3-touch):**
- **Email announcement** (to all active clients): subject "A little 'thank you' for the friends you've been sending our way" — explains the new mechanic and includes the client's personal code.
- **In-chair script** (providers, at checkout): "Have you tried our new Bring a Friend program? You get $30 credit every time a friend books their first facial with your code."
- **Social post** (IG/FB): photo of two clients enjoying robe-and-tea moment, caption explaining the mechanic, link in bio.

**Optimization Levers:**
- **Not enough referrals?** Run a "double reward weekend" where both sides get $50 instead of $30 (4 weekends per year max).
- **Too many referrals eating margin?** Cap monthly earned credits to $100 per referrer; restrict friend offer to off-peak days only.
- **Low conversion on friend's first visit?** Add a booking-time reminder text with what to expect at the appointment.

**Risk & Compliance Notes:**
- Pure day spa services are unrestricted in most US jurisdictions; if the spa adds medical services (injectables, IV therapy, laser), check state rules before offering cash incentives for those services.
- All SMS shares must be opt-in — the business sends the invite to the referrer, not directly to the friend.
- Spell out the terms on the website footer to avoid disputes over expired credits.

**Economics Check:**
- Reward cost per successful referral: ~$60 ($30 to friend via discount, $30 credit to referrer).
- Target CAC: $40 — referral CAC is higher per new client, but LTV is also higher (referred clients have ~30% higher annual retention industry-wide).
- Break-even: New client must complete ~1.5 additional visits beyond the first to cover the program cost at normal ticket size. Most referred clients exceed this within their first 6 months.
