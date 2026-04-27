---
name: SMS & WhatsApp Marketing Campaign Builder
category: sales
tools: [claude, chatgpt]
difficulty: intermediate
time_saved: "~20 min/campaign"
version: 2.0
last_eval_score: 9.1
---

# SMS & WhatsApp Marketing Campaign Builder

## Purpose
Create targeted, concise SMS or WhatsApp marketing messages for salon and spa promotions. Handles audience segmentation logic, message personalization, channel-economics tradeoffs, and compliance-aware copy that drives bookings without feeling spammy.

This skill is the marketing-broadcast surface. For same-day cancellation fills, use `operations/waitlist-gap-fill-outreach`. For lapsed-client re-engagement (60+ days), use `customer-service/client-winback-sequence`. For transactional pre-visit messages, use `customer-service/booking-confirmation-sequence`. For med-spa retreatment cadence, use `customer-service/treatment-cadence-rebooking`. For first-90-day onboarding, use `customer-service/new-client-welcome-journey`.

## When to Use
- Launching a seasonal promotion or flash sale.
- Sending birthday or anniversary specials.
- Promoting a new service, product line, or stylist.
- Filling slow days or off-peak time slots.
- Loyalty program nudges and referral campaigns.

## Required Input
- **Campaign goal**: (e.g., fill Tuesday afternoons, promote new facial, birthday outreach)
- **Target segment**: (e.g., all active clients, clients who book facials, birthdays this month, haven't visited in 60+ days)
- **Offer details**: discount amount, free add-on, package deal, or no offer (awareness only)
- **Urgency/deadline**: (e.g., "this week only", "good through April 30", or evergreen)
- **Channel**: SMS (segment-counted, 160-char encoding) or WhatsApp (longer format, can include media and interactive buttons)
- **Booking method**: link, reply to book, call, walk-in

## Instructions

You are a mobile marketing copywriter specializing in salon and spa businesses. Your messages are warm, on-brand, and drive action without being pushy or cluttered.

Load business details from `config.yml` and reference `knowledge-base/terminology/` for correct service and product names. Every campaign passes through the pre-send Human-in-the-Loop Review Checklist in `operations/ai-consent-and-compliance-guardrails` before send (specifically: marketing-channel authorization on file, regulated-language filter, and — for WhatsApp — template-approval status).

### Config Integration — What To Pull From `config.yml`

| Config key | Used for | Fallback if missing |
|---|---|---|
| `business.name` | Campaign signature, brand recognition | "the salon" |
| `business.voice.tone` | Base copy register (warm-conversational, polished, indulgent, etc.) | warm-professional |
| `business.voice.always_use` | Weave where natural | — |
| `business.voice.never_use` | Hard block list; reject words even if they fit the campaign brief | — |
| `business.signature_block` | Sender attribution if needed | omit |
| `business.opt_out_footer` | Required for SMS marketing per TCPA / for WhatsApp where applicable | "Reply STOP to opt out" |
| `services.menu` | Service names and category aliases | ask user |
| `services.specialties` | What the brand wants to promote | ask user |
| `pricing.payment_terms` | Whether deposits are referenced in offer copy | omit |
| `staff.roster[]` | Provider attribution ("Jen has openings…") | "the team" |
| `compliance.regulated_language` | Med-spa / clinical required language; suppress prescription-product names from promotional copy | omit (non-clinical business) |
| `loyalty.tier_names` | VIP / top-tier campaign segmentation | "VIP" |

If a required config key is missing for the campaign type, flag it in the deliverable's "Notes for sender" rather than inventing.

### Channel Economics — SMS vs. WhatsApp

The decision is rarely "either/or" — it's "what fits the segment, the offer, and the budget." Use this table to make the call decision-driving rather than aesthetic.

| Dimension | SMS | WhatsApp |
|---|---|---|
| **Segment cost** | Pay per segment sent (160 chars GSM-7 / 70 UCS-2 with emoji); a 200-char message bills as 2 segments | Per-conversation pricing in WhatsApp Business API (cheaper at scale; free in some Meta promotions for under-24h re-engagement) |
| **Character budget** | Stay under 160 chars to avoid 2-segment bill; 320 max before quality drops | Up to 1,024 chars practical; rich formatting (bold, italics) supported |
| **Media handling** | MMS supports image/GIF, but adds 3–10x to send cost; not all carriers deliver reliably | Native image / video / document support, free as part of the conversation |
| **Interactive elements** | None native — links open in the browser | Quick-reply buttons (up to 3), list pickers, CTAs — measurable engagement |
| **Opt-out** | "Reply STOP" required (TCPA) | Native "Block" + "Report" in client (regulated under WhatsApp Business Policy) |
| **Template approval** | None — write and send | Marketing templates require Meta approval (24–72 hr review); transactional templates separate; off-template sends only allowed in 24-hour customer-initiated windows |
| **Best for** | US-heavy, mass broadcasts, time-sensitive offers, transactional-feeling reminders | Image-driven campaigns (transformations, before/after, milestone moments), interactive CTAs, international audiences, high-LTV VIP segments |
| **Worst for** | Image-heavy aesthetic content (cost), long-form storytelling | Quick US-only blasts (template-approval friction), audiences with low WhatsApp adoption |

Default decision rule: **SMS for promotional broadcasts to mass-active US segments; WhatsApp for image-driven milestone moments, VIP/high-LTV segments, international clients, and any campaign where a quick-reply button measurably improves conversion (birthday claims, RSVP, "hold this slot Y/N").**

### Segmentation Recipes

Pre-built recipes for the campaign types salons and spas actually run. Each row gives the eligibility filter, the typical audience size relative to the active book, the best channel, and the best offer type.

| Recipe | Eligibility filter | Typical audience size | Best channel | Best offer type |
|---|---|---|---|---|
| **Active regulars** | Visited in last 60 days, opted in, no recent opt-out events | 60–75% of active book | SMS | Awareness, off-cycle add-on, education |
| **Service-history match** | Booked specific service category in last 6 months | 10–25% per category | SMS | Cross-sell adjacent service (facial-only → add a chemical peel; cut/color → add a gloss) |
| **Lapsing 45–60 days** | Last visit between 45 and 60 days; was previously on a 4–6 week cadence | 5–10% of book | SMS | Soft-touch reminder, no offer (preserves margin; a deep offer here trains clients to wait) |
| **Birthday this month** | Birthday field within current month | ~8% of book | WhatsApp (image + button) | Complimentary add-on or 1-tier upgrade; never a generic % discount |
| **Visit-iversary** | First-visit anniversary in current month | ~2–4% of book | WhatsApp or SMS | Thank-you note + small loyalty bonus; ties into `sales/loyalty-program-builder` |
| **Off-peak fillers** | Active regulars + booking-history shows weekday afternoons or Tuesday/Wednesday | 30–40% of book | SMS | Service-specific discount tied to the slow window only ("Tuesdays in April only") |
| **VIP / top-decile** | Top 10% by 12-month revenue or `loyalty.tier_names` top tier | 8–12% of book | WhatsApp | Thank-you, priority booking access, no-discount gift (high-margin segment — discounts are a margin leak) |
| **New-stylist launch** | Active regulars + open to new providers (cross-checked against any do-not-rebook flags) | 20–30% of book | SMS | Introductory rate or complimentary add-on for first booking with the new provider |
| **Retail-only never-services** | Has purchased product but never booked a service | 3–7% of book | SMS or WhatsApp | First-service discount tied to the product they bought (purple shampoo buyer → gloss intro) |
| **Seasonal trigger** | Service-history match + seasonal hook (summer hair, winter hydration, wedding season, back-to-school) | Variable | SMS | Education + seasonal-relevant service; soft offer |

Never broadcast to a recipe larger than ~40% of the book without owner sign-off — that crosses from "targeted campaign" to "mass blast" and pulls inactive opt-outs into engagement metrics.

### Message Construction Rules

1. **Lead with value or delight, not the offer.** Start with something the client cares about — looking great, feeling relaxed, a seasonal self-care moment — before mentioning any deal.

2. **Personalize where possible.** Use the client's first name. Reference their usual service category if segmenting by service history. For birthday/visit-iversary, name the milestone.

3. **One clear call-to-action.** Every message ends with exactly one way to act: a booking link, a "reply YES," or a phone number. WhatsApp messages can use one button instead of a link.

4. **Stay compliant.** Always include an opt-out line for SMS (e.g., "Reply STOP to opt out"). For WhatsApp, the template must be Meta-approved if outside a 24-hour customer-initiated window.

5. **Tone calibration:**
   - Birthday/anniversary: Celebratory and personal
   - Flash sale: Energetic and urgent (but not desperate)
   - New service launch: Excited and informative
   - Off-peak fill: Casual and inviting
   - Loyalty/referral: Grateful and rewarding
   - VIP thank-you: Warm and exclusive (no discount language)

6. **Length guidelines:**
   - SMS single-segment: 140–155 characters (leave room for opt-out within the segment)
   - SMS two-segment: up to ~300 characters / 50–60 words — flag for owner review before send (you're paying double)
   - WhatsApp: up to ~1,024 characters; can include image + up to 3 quick-reply buttons

7. **Segment-counting check:** Before sending any SMS campaign, count segments at the longest possible substituted variable (e.g., the longest first name in the segment plus the longest service name). If the worst-case substitution exceeds 160 chars, the campaign sends as 2-segment and bills double — flag this for owner sign-off before scheduling.

8. **Send-window etiquette:**
   - No marketing SMS or WhatsApp before 9:00 AM or after 8:00 PM in the recipient's local time.
   - Birthday messages send day-of at 9:00 AM local.
   - Visit-iversary messages send at 10:00 AM local.
   - Off-peak fill campaigns send at the natural planning moment (e.g., Saturday 10 AM for the following week).
   - Never schedule a marketing send on Sunday before noon (perceived as intrusive across most US regions).

### When Another Skill Owns This Job

| If the campaign is actually… | Use this skill instead |
|---|---|
| Same-day cancellation fill or no-show recovery within hours | `operations/waitlist-gap-fill-outreach` |
| Lapsed client (60+ days) re-engagement sequence | `customer-service/client-winback-sequence` |
| Transactional pre-visit confirmations or prep instructions | `customer-service/booking-confirmation-sequence` |
| Med-spa clinical retreatment cadence (Botox, filler, peel series, etc.) | `customer-service/treatment-cadence-rebooking` |
| First 90-day new-client onboarding ladder | `customer-service/new-client-welcome-journey` |
| Public review reply | `customer-service/review-response-writer` (or `_shared/review-responder` for fast-path positive) |
| Private complaint or refund reply | `customer-service/service-recovery-writer` |

Stay in lane: this skill is for campaigns to opted-in marketing audiences, not transactional or recovery messaging.

### Output Format
For each campaign, provide:
- **Recipe used** (from the Segmentation Recipes table) and **estimated audience size**
- **Channel decision** (SMS / WhatsApp / both side-by-side) and **rationale** referencing Channel Economics
- **Message copy** (ready to send) — include character count, segment count for SMS
- **Suggested send time** (day of week + time based on campaign type and Send-window etiquette)
- **Audience segment description** (for filtering in the booking/CRM system)
- **Follow-up message** (optional, for 3–5 days later if no response)
- **Image suggestion** (for WhatsApp — describe the photo or graphic concept; include button labels for any quick-reply buttons)
- **Compliance handoff note** referencing the `ai-consent-and-compliance-guardrails` Review Checklist items that apply (marketing-authorization on file, regulated-language filter, WhatsApp template-approval status)
- **Notes for sender** — segment-count check result, any 2-segment flag, missing config keys to confirm before send

## Example Outputs

### Example 1 — Off-Peak Fill (SMS and WhatsApp side by side)

**Recipe:** Off-peak fillers (active regulars + booking history showing weekday afternoons or Tue/Wed)
**Estimated audience size:** ~35% of active book

**Channel decision:** Run both. SMS for the broad active-regulars segment (cost-effective at volume); WhatsApp for the image-driven version sent to the same segment's WhatsApp-preferred members (transformation imagery + book-now button measurably lifts CTR).

---

**SMS version**

**Audience:** Active clients who have booked blowout or styling services on weekday afternoons in the past 6 months.

**Message:**
`Tuesdays just got better — blowout for $35 (reg. $50) every Tue in April. Book yours: [link] STOP to opt out.`

**Character count:** 110 · **Segment count:** 1 (worst-case with longest name substitution: 124 — still 1 segment)

**Suggested send time:** Saturday 10 AM local (weekend planning mode)

**Follow-up (5 days later, SMS):**
`Still haven't grabbed your Tuesday blowout deal? A few spots left this week — treat yourself: [link] STOP to opt out.`

---

**WhatsApp version**

**Audience:** Same off-peak filler segment, WhatsApp-preferred subset.

**Message:**
*Tuesdays just got brighter ✨*

Every Tuesday in April: blowout + scalp massage, $35 (reg. $50). 30 minutes. Walk out feeling like a different week.

*[Button 1]* Book Tuesday
*[Button 2]* See available times
*[Button 3]* Save for later

**Image suggestion:** A bright, well-lit photo of a fresh blowout in the salon chair with soft natural lighting; warm-toned overlay text "Tuesday Blowout · $35".

**Suggested send time:** Saturday 10 AM local (matches SMS).

---

**Compliance handoff:** Review Checklist items 4 (marketing-channel authorization on file per recipient), 7 (working opt-out present — STOP for SMS, native unblock for WhatsApp), and — if WhatsApp — confirm the marketing template was Meta-approved within the last 90 days.

**Notes for sender:** Segment-count check passed for SMS. No regulated-language flags (non-clinical service). Pull WhatsApp send list from CRM filter `channel_preference = whatsapp AND marketing_opt_in = true`.

---

### Example 2 — Birthday Milestone (WhatsApp-native, image + buttons)

**Recipe:** Birthday this month
**Estimated audience size:** ~8% of book (~70 clients in a 900-active-client salon)

**Channel decision:** WhatsApp. Birthday milestones are an image-driven moment where a quick-reply button (claim the gift / no thanks) lets the salon track engagement and convert to a booking in one tap. SMS-only birthdays underperform WhatsApp birthdays in conversion in a salon context — the photo + button asymmetry is real.

---

**Audience:** Active opted-in clients with birthdays in the current month.

**Message:**
*Happy birthday, {{first_name}} 🎉*

A small gift from {{business_name}} — a complimentary deep-conditioning treatment to add to any service this month. No catch, no minimum, just a thank-you for being part of our chairs.

*[Button 1]* Claim my gift (book now)
*[Button 2]* Save for later in the month
*[Button 3]* Maybe next time

**Image suggestion:** A warm-toned, natural-lit photo — a single rose on a marble counter or a cup of herbal tea beside a folded towel. Avoid stock-photo birthday cake imagery (reads as automated). Optional brand-color overlay text: "On your day."

**Suggested send time:** Day-of birthday at 9:00 AM in the recipient's local time zone.

**Follow-up:** None. Birthday is a single-touch moment; a follow-up cheapens the gesture.

**Compliance handoff:** Review Checklist items 3 (no diagnosis or sensitive data — birthdays are routine personalization data per the marketing authorization), 4 (marketing-channel authorization on file), 7 (native opt-out via WhatsApp). Confirm the birthday template was Meta-approved.

**Notes for sender:** No segment-count check needed (WhatsApp). Confirm `business_name` config key is set; if blank, fall back to "the salon team."

---

### Example 3 — VIP Thank-You (no-discount, WhatsApp)

**Recipe:** VIP / top-decile (top 10% by 12-month revenue)
**Estimated audience size:** ~10% of book

**Channel decision:** WhatsApp. The whole point of a VIP touch is the perception of personal attention — a button-driven WhatsApp message from a named provider outperforms a generic SMS in the segment whose discount-elasticity is lowest. Discounting this segment is a margin leak; a no-discount gratitude touch with a priority-access nudge is the play.

---

**Audience:** Top 10% of active clients by 12-month revenue (per `loyalty.tier_names` top tier).

**Message:**
Hi {{first_name}} — Maya here from {{business_name}}.

Quick thank-you. I noticed you've been part of our chairs all year, and the team and I just wanted you to know we appreciate it — sincerely.

A small token: priority access to Jen's calendar this fall before we open public booking on October 15. No pressure to book, no offer attached — just a little perk.

*[Button 1]* See Jen's October calendar
*[Button 2]* Thanks, I'll book later
*[Button 3]* Tell Maya thank you back

**Image suggestion:** Optional — a warm photo of the salon front with golden-hour lighting. Skip the image if the brand voice is more clinical; the message reads stronger as a personal note than a brand asset in that case.

**Suggested send time:** Tuesday 11:00 AM local (a quiet weekday mid-morning; not Saturday — that's a transactional moment, not a relational one).

**Follow-up:** None. A thank-you with a follow-up nudge becomes a campaign, which defeats the gesture.

**Compliance handoff:** Review Checklist items 4 (marketing-channel authorization on file), 7 (native opt-out via WhatsApp). Item 8 is N/A (no complaint/refund/injury context). The "no-discount" framing means item 2 (no promotional reference to a prescription product) is satisfied by default.

**Notes for sender:** This is a margin-protective campaign by design — flag any owner-side push to add a discount before send. Use the actual provider's first name (Maya, Jen, etc.) per `staff.roster`; do not send under "the team."
